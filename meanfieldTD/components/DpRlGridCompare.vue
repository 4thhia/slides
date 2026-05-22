<template>
  <div class="dp-rl-wrap">
    <section class="panel">
      <div class="panel-head">
        <h2>{{ dpPanelTitle }}</h2>
      </div>

      <div class="grid-area">
        <div class="grid-wrap">
          <svg class="arrow-layer" :width="gridPixelSize" :height="gridPixelSize" viewBox="0 0 231 231">
            <defs>
              <marker id="arrowhead" markerWidth="4" markerHeight="4" refX="3" refY="1.5" orient="auto">
                <path d="M0,0 L0,3 L3,1.5 z" fill="rgba(160, 120, 255, 0.72)" />
              </marker>
            </defs>

            <line
              v-for="(seg, idx) in dpArrowSegments"
              :key="`dp-arrow-${idx}`"
              :x1="seg.x1"
              :y1="seg.y1"
              :x2="seg.x2"
              :y2="seg.y2"
              class="arrow"
              marker-end="url(#arrowhead)"
            />
          </svg>

          <div class="grid">
            <div
              v-for="cell in cells"
              :key="`dp-${cell.x}-${cell.y}`"
              class="cell"
              :class="dpCellClass(cell)"
            >
              <div class="value" :class="{ label: dpStage === -1 && (isStart(cell) || isGoal(cell)) }">
                {{ dpCellText(cell) }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="controls">
        <button @click="prevDp">Prev</button>
        <button @click="nextDp">Next</button>
        <button @click="resetDp">Reset</button>
      </div>
    </section>

    <section class="panel">
      <div class="panel-head">
        <h2>{{ rlPanelTitle }}</h2>
      </div>

      <div class="grid-area">
        <div class="grid-wrap">
          <svg class="arrow-layer" :width="gridPixelSize" :height="gridPixelSize" viewBox="0 0 231 231">
            <defs>
              <marker id="arrowhead-rl" markerWidth="4" markerHeight="4" refX="3" refY="1.5" orient="auto">
                <path d="M0,0 L0,3 L3,1.5 z" fill="rgba(160, 120, 255, 0.72)" />
              </marker>
            </defs>

            <line
              v-for="(seg, idx) in rlArrowSegments"
              :key="`rl-arrow-${idx}`"
              :x1="seg.x1"
              :y1="seg.y1"
              :x2="seg.x2"
              :y2="seg.y2"
              class="arrow"
              marker-end="url(#arrowhead-rl)"
            />
          </svg>

          <div class="grid">
            <div
              v-for="cell in cells"
              :key="`rl-${cell.x}-${cell.y}`"
              class="cell"
              :class="rlCellClass(cell)"
            >
              <div class="value" :class="{ label: rlStep === 0 && (isStart(cell) || isGoal(cell)) }">
                {{ rlCellText(cell) }}
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="controls">
        <button @click="prevRl">Prev</button>
        <button @click="nextRl">Next</button>
        <button @click="resetRl">Reset</button>
      </div>
    </section>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const EPS = 1e-9;
const SIZE = 6;
const GOAL = SIZE - 1;
const CELL_SIZE = 36;
const CELL_GAP = 3;
const GRID_PIXEL_SIZE = SIZE * CELL_SIZE + (SIZE - 1) * CELL_GAP;

const START = { x: 0, y: 0 };
const DP_FOCUS_1 = { x: 1, y: 3 };
const DP_FOCUS_2 = { x: 2, y: 2 };
const DP_BELLMAN_TARGET = { x: 1, y: 2 };

const STEP_REWARD = 1;
const GAMMA = 1;

const NUM_MC_PATHS = 5;
const NUM_TD_PATHS = 10;
const MC_PHASES_PER_PATH = 4;

const gridPixelSize = GRID_PIXEL_SIZE;

const dpStages = [-1, 0, 0.5, 1, 2, 3, 3.5, 4, 5, 6, 6.5, 7, 7.5, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30];
const dpStageIndex = ref(0);
const dpStage = computed(() => dpStages[dpStageIndex.value]);

const dpPanelTitle = computed(() => {
  if (dpStage.value >= 9 && dpStage.value <= 18) {
    return 'Dynamic Programming (backward induction)';
  }

  if (dpStage.value >= 19) {
    return 'Dynamic Programming (value iteration)';
  }

  return 'Dynamic Programming';
});

const rlStep = ref(0);

const makeValues = () => {
  const values = Array.from({ length: SIZE }, () => Array.from({ length: SIZE }, () => 0));
  values[GOAL][GOAL] = 0;
  return values;
};

const generateRandomPath = () => {
  const path = [{ x: 0, y: 0 }];
  let x = 0;
  let y = 0;

  while (x < GOAL || y < GOAL) {
    if (x === GOAL) {
      y += 1;
    } else if (y === GOAL) {
      x += 1;
    } else if (Math.random() < 0.5) {
      x += 1;
    } else {
      y += 1;
    }

    path.push({ x, y });
  }

  return path;
};

const generateRandomPaths = (count) => {
  return Array.from({ length: count }, () => generateRandomPath());
};

const cells = computed(() => {
  const out = [];
  for (let y = GOAL; y >= 0; y -= 1) {
    for (let x = 0; x < SIZE; x += 1) {
      out.push({ x, y });
    }
  }
  return out;
});

const dpValues = ref(makeValues());
const rlValues = ref(makeValues());

const mcPaths = ref(generateRandomPaths(NUM_MC_PATHS));
const tdPaths = ref(generateRandomPaths(NUM_TD_PATHS));

const sameCell = (a, b) => a.x === b.x && a.y === b.y;

const isGoal = (cell) => cell.x === GOAL && cell.y === GOAL;
const isStart = (cell) => cell.x === START.x && cell.y === START.y;
const isDpFocus1 = (cell) => sameCell(cell, DP_FOCUS_1);
const isDpFocus2 = (cell) => sameCell(cell, DP_FOCUS_2);
const isDpBellmanTarget = (cell) => sameCell(cell, DP_BELLMAN_TARGET);

const dpCellText = (cell) => {
  if (dpStage.value === -1) {
    if (isStart(cell)) return 'Start';
    if (isGoal(cell)) return 'Goal';
    return '';
  }

  return formatValue(dpValues.value[cell.y][cell.x]);
};

const rlCellText = (cell) => {
  if (rlStep.value === 0) {
    if (isStart(cell)) return 'Start';
    if (isGoal(cell)) return 'Goal';
    return '';
  }

  return formatValue(rlValues.value[cell.y][cell.x]);
};

const setCell = (values, x, y, v) => {
  values[y][x] = v;
};

const setMany = (values, cells, v) => {
  cells.forEach((cell) => {
    values[cell.y][cell.x] = v;
  });
};

const manhattanToGoal = (cell) => {
  return (GOAL - cell.x) + (GOAL - cell.y);
};

const diagonalCells = (distance) => {
  const out = [];
  for (let y = 0; y < SIZE; y += 1) {
    for (let x = 0; x < SIZE; x += 1) {
      if (manhattanToGoal({ x, y }) === distance) out.push({ x, y });
    }
  }
  return out;
};

const applyManualDiagonalSteps = (values, stage) => {
  const maxDistance = 2 * GOAL;
  const lastDistance = Math.min(stage - 8, maxDistance);

  for (let distance = 1; distance <= lastDistance; distance += 1) {
    setMany(values, diagonalCells(distance), distance);
  }
};

const synchronousDpSweep = (oldValues) => {
  const nextValues = makeValues();

  for (let y = 0; y < SIZE; y += 1) {
    for (let x = 0; x < SIZE; x += 1) {
      const cell = { x, y };

      if (isGoal(cell)) {
        nextValues[y][x] = 0;
        continue;
      }

      const successors = [];
      if (x < GOAL) successors.push({ x: x + 1, y });
      if (y < GOAL) successors.push({ x, y: y + 1 });

      const total = successors.reduce((acc, s) => acc + oldValues[s.y][s.x], 0);
      nextValues[y][x] = STEP_REWARD + total / successors.length;
    }
  }

  nextValues[GOAL][GOAL] = 0;
  return nextValues;
};

const applySynchronousDpSweeps = (values, count) => {
  let current = values;
  for (let i = 0; i < count; i += 1) {
    current = synchronousDpSweep(current);
  }
  return current;
};

const recomputeDp = (stage) => {
  let values = makeValues();

  if (stage >= 2 && stage < 8) {
    setCell(values, 1, 3, 6);
  }

  if (stage >= 5 && stage < 8) {
    setCell(values, 2, 2, 6);
  }

  if (stage >= 7.5 && stage < 8) {
    setCell(values, 1, 2, 7);
  }

  if (stage >= 9 && stage <= 18) {
    applyManualDiagonalSteps(values, stage);
  }

  if (stage >= 19) {
    values = applySynchronousDpSweeps(makeValues(), stage - 19);
  }

  dpValues.value = values;
};

const applyMonteCarloReturnUpdate = (oldValues, path) => {
  const nextValues = oldValues.map((row) => [...row]);
  let returnValue = 0;

  for (let i = path.length - 2; i >= 0; i -= 1) {
    const cell = path[i];
    returnValue = STEP_REWARD + GAMMA * returnValue;
    nextValues[cell.y][cell.x] = returnValue;
  }

  nextValues[GOAL][GOAL] = 0;
  return nextValues;
};

const applyOneTdTransitionUpdate = (oldValues, transition) => {
  const nextValues = oldValues.map((row) => [...row]);
  const current = transition.from;
  const next = transition.to;

  nextValues[current.y][current.x] = STEP_REWARD + GAMMA * oldValues[next.y][next.x];
  nextValues[GOAL][GOAL] = 0;

  return nextValues;
};

const tdTransitions = computed(() => {
  const transitions = [];

  for (let pathIndex = 0; pathIndex < tdPaths.value.length; pathIndex += 1) {
    const path = tdPaths.value[pathIndex];

    for (let i = 0; i < path.length - 1; i += 1) {
      transitions.push({
        pathIndex,
        transitionIndex: i,
        from: path[i],
        to: path[i + 1],
      });
    }
  }

  return transitions;
});

const mcStepCount = computed(() => mcPaths.value.length * MC_PHASES_PER_PATH);
const rlResetStep = computed(() => mcStepCount.value + 2);
const tdStartStep = computed(() => rlResetStep.value + 1);
const maxRlStep = computed(() => mcStepCount.value + 2 + tdTransitions.value.length);

const isMcPhase = computed(() => rlStep.value >= 2 && rlStep.value <= mcStepCount.value + 1);
const isResetPhase = computed(() => rlStep.value === rlResetStep.value);
const isTdPhase = computed(() => rlStep.value >= tdStartStep.value && rlStep.value <= maxRlStep.value);

const rlPanelTitle = computed(() => {
  if (isMcPhase.value) {
    return 'Reinforcement Learning (Monte Carlo Method)';
  }

  if (isTdPhase.value) {
    return 'Reinforcement Learning (TD learning)';
  }

  return 'Reinforcement Learning';
});

const currentMcPathIndex = computed(() => {
  if (!isMcPhase.value) return -1;
  return Math.floor((rlStep.value - 2) / MC_PHASES_PER_PATH);
});

const currentMcPhase = computed(() => {
  if (!isMcPhase.value) return 0;
  return ((rlStep.value - 2) % MC_PHASES_PER_PATH) + 1;
});

const currentMcPath = computed(() => {
  if (currentMcPathIndex.value < 0 || currentMcPathIndex.value >= mcPaths.value.length) return [];
  return mcPaths.value[currentMcPathIndex.value];
});

const mcAppliedUpdateCount = computed(() => {
  if (!isMcPhase.value) return 0;

  const pathIndex = currentMcPathIndex.value;
  const phase = currentMcPhase.value;
  const count = pathIndex + (phase >= 3 ? 1 : 0);

  return Math.min(count, mcPaths.value.length);
});

const currentTdTransitionIndex = computed(() => {
  if (!isTdPhase.value) return -1;
  return rlStep.value - tdStartStep.value;
});

const currentTdTransition = computed(() => {
  if (currentTdTransitionIndex.value < 0 || currentTdTransitionIndex.value >= tdTransitions.value.length) return null;
  return tdTransitions.value[currentTdTransitionIndex.value];
});

const recomputeRl = () => {
  let values = makeValues();

  if (rlStep.value === 0 || rlStep.value === 1) {
    rlValues.value = makeValues();
    return;
  }

  if (isMcPhase.value) {
    for (let pathIndex = 0; pathIndex < mcAppliedUpdateCount.value; pathIndex += 1) {
      values = applyMonteCarloReturnUpdate(values, mcPaths.value[pathIndex]);
    }

    rlValues.value = values;
    return;
  }

  if (isResetPhase.value) {
    rlValues.value = makeValues();
    return;
  }

  if (isTdPhase.value) {
    const lastTransitionIndex = currentTdTransitionIndex.value;

    for (let i = 0; i <= lastTransitionIndex; i += 1) {
      values = applyOneTdTransitionUpdate(values, tdTransitions.value[i]);
    }

    rlValues.value = values;
    return;
  }

  rlValues.value = makeValues();
};

const cellCenter = (cell) => {
  return {
    x: cell.x * (CELL_SIZE + CELL_GAP) + CELL_SIZE / 2,
    y: (GOAL - cell.y) * (CELL_SIZE + CELL_GAP) + CELL_SIZE / 2,
  };
};

const shortArrow = (from, to) => {
  const a = cellCenter(from);
  const b = cellCenter(to);
  const dx = b.x - a.x;
  const dy = b.y - a.y;
  const length = Math.sqrt(dx * dx + dy * dy);
  const offset = 10;

  if (length === 0) {
    return { x1: a.x, y1: a.y, x2: b.x, y2: b.y };
  }

  const ux = dx / length;
  const uy = dy / length;

  return {
    x1: a.x + ux * offset,
    y1: a.y + uy * offset,
    x2: b.x - ux * offset,
    y2: b.y - uy * offset,
  };
};

const pushFeedbackArrowsForCell = (segments, cell) => {
  if (cell.x < GOAL) {
    segments.push(shortArrow({ x: cell.x + 1, y: cell.y }, cell));
  }

  if (cell.y < GOAL) {
    segments.push(shortArrow({ x: cell.x, y: cell.y + 1 }, cell));
  }
};

const isInMonotoneRegionFrom = (cell, start) => {
  return cell.x >= start.x && cell.y >= start.y && cell.x <= GOAL && cell.y <= GOAL;
};

const dpHighlightStart = computed(() => {
  if (dpStage.value === 1 || dpStage.value === 2) return DP_FOCUS_1;
  if (dpStage.value === 4 || dpStage.value === 5) return DP_FOCUS_2;
  return null;
});

const dpArrowSegments = computed(() => {
  const segments = [];

  if (dpStage.value === 7) {
    segments.push(shortArrow({ x: 1, y: 3 }, DP_BELLMAN_TARGET));
    segments.push(shortArrow({ x: 2, y: 2 }, DP_BELLMAN_TARGET));
    return segments;
  }

  if (dpStage.value >= 9 && dpStage.value <= 18) {
    const distance = dpStage.value - 8;
    const targets = diagonalCells(distance);

    for (const cell of targets) {
      pushFeedbackArrowsForCell(segments, cell);
    }

    return segments;
  }

  if (dpStage.value >= 20 && dpStage.value <= 30) {
    for (let y = 0; y < SIZE; y += 1) {
      for (let x = 0; x < SIZE; x += 1) {
        const cell = { x, y };
        if (isGoal(cell)) continue;
        pushFeedbackArrowsForCell(segments, cell);
      }
    }

    return segments;
  }

  return segments;
});

const formatValue = (v) => {
  return v.toFixed(1);
};

const nextDp = () => {
  if (dpStageIndex.value >= dpStages.length - 1) return;
  dpStageIndex.value += 1;
  recomputeDp(dpStage.value);
};

const prevDp = () => {
  if (dpStageIndex.value <= 0) return;
  dpStageIndex.value -= 1;
  recomputeDp(dpStage.value);
};

const resetDp = () => {
  dpStageIndex.value = 0;
  dpValues.value = makeValues();
};

const nextRl = () => {
  if (rlStep.value >= maxRlStep.value) return;
  rlStep.value += 1;
  recomputeRl();
};

const prevRl = () => {
  if (rlStep.value <= 0) return;
  rlStep.value -= 1;
  recomputeRl();
};

const resetRl = () => {
  rlStep.value = 0;
  rlValues.value = makeValues();
};

const dpSweepStatus = computed(() => {
  if (dpStage.value < 20) return null;

  const before = applySynchronousDpSweeps(makeValues(), dpStage.value - 20);
  const after = synchronousDpSweep(before);

  const changed = new Set();
  const unchanged = new Set();

  for (let y = 0; y < SIZE; y += 1) {
    for (let x = 0; x < SIZE; x += 1) {
      const key = `${x},${y}`;

      if (Math.abs(after[y][x] - before[y][x]) <= EPS) {
        unchanged.add(key);
      } else {
        changed.add(key);
      }
    }
  }

  return { changed, unchanged };
});

const dpCellClass = (cell) => {
  const shouldHighlight = dpHighlightStart.value && isInMonotoneRegionFrom(cell, dpHighlightStart.value);

  const isFocusPreview =
    (dpStage.value === 0.5 && isDpFocus1(cell)) ||
    (dpStage.value === 1 && isDpFocus1(cell)) ||
    (dpStage.value === 3.5 && isDpFocus2(cell)) ||
    (dpStage.value === 4 && isDpFocus2(cell)) ||
    (dpStage.value === 6.5 && isDpBellmanTarget(cell)) ||
    (dpStage.value === 7 && isDpBellmanTarget(cell));

  const isComputedByPathEnumeration =
    (dpStage.value === 2 && isDpFocus1(cell)) ||
    (dpStage.value === 5 && isDpFocus2(cell));

  const isBellmanTarget =
    dpStage.value >= 7.5 && dpStage.value < 8 && isDpBellmanTarget(cell);

  const isBackwardUpdateTarget =
    dpStage.value >= 9 && dpStage.value <= 18 && manhattanToGoal(cell) === dpStage.value - 8;

  const key = `${cell.x},${cell.y}`;
  const isSweepChanged = dpSweepStatus.value?.changed.has(key) ?? false;
  const isSweepUnchanged = dpSweepStatus.value?.unchanged.has(key) ?? false;

  return {
    start: isStart(cell),
    goal: isGoal(cell),
    focus: isFocusPreview,
    path: shouldHighlight,
    active:
      isComputedByPathEnumeration ||
      isBellmanTarget ||
      isBackwardUpdateTarget ||
      isSweepChanged,
    stable: dpStage.value >= 20 && (isGoal(cell) || isSweepUnchanged),
    sweep: dpStage.value >= 19 && dpStage.value <= 30,
  };
};

const computeRlStableMask = (values) => {
  const stable = Array.from({ length: SIZE }, () => Array.from({ length: SIZE }, () => false));

  stable[GOAL][GOAL] = true;

  for (let distance = 1; distance <= 2 * GOAL; distance += 1) {
    const cellsOnDiagonal = diagonalCells(distance);

    for (const cell of cellsOnDiagonal) {
      const valueMatchesTruth = Math.abs(values[cell.y][cell.x] - manhattanToGoal(cell)) <= EPS;
      const rightStable = cell.x === GOAL || stable[cell.y][cell.x + 1];
      const upStable = cell.y === GOAL || stable[cell.y + 1][cell.x];

      stable[cell.y][cell.x] = valueMatchesTruth && rightStable && upStable;
    }
  }

  return stable;
};

const rlStableMask = computed(() => computeRlStableMask(rlValues.value));

const rlPathVisibleCells = computed(() => {
  if (isMcPhase.value && currentMcPhase.value >= 1 && currentMcPhase.value <= 3) {
    return currentMcPath.value;
  }

  if (isTdPhase.value && currentTdTransition.value) {
    return [currentTdTransition.value.from, currentTdTransition.value.to];
  }

  return [];
});

const rlActiveCells = computed(() => {
  if (isMcPhase.value && currentMcPhase.value === 3) {
    return currentMcPath.value.slice(0, -1);
  }

  if (isTdPhase.value && currentTdTransition.value) {
    return [currentTdTransition.value.from];
  }

  return [];
});

const rlArrowSegments = computed(() => {
  const segments = [];

  if (isMcPhase.value && (currentMcPhase.value === 2 || currentMcPhase.value === 3)) {
    const path = currentMcPath.value;

    for (let i = path.length - 1; i >= 1; i -= 1) {
      segments.push(shortArrow(path[i], path[i - 1]));
    }

    return segments;
  }

  if (isTdPhase.value && currentTdTransition.value) {
    segments.push(shortArrow(currentTdTransition.value.to, currentTdTransition.value.from));
    return segments;
  }

  return segments;
});

const rlCellClass = (cell) => {
  const isPathVisible = rlPathVisibleCells.value.some((p) => sameCell(p, cell));
  const isActive = rlActiveCells.value.some((p) => sameCell(p, cell));

  const isStable = rlStableMask.value[cell.y][cell.x];
  const shouldShowStable = rlStep.value >= 2 && !isResetPhase.value && isStable && !isPathVisible;

  return {
    start: isStart(cell),
    goal: isGoal(cell),
    path: isPathVisible,
    active: isActive,
    stable: shouldShowStable,
  };
};
</script>

<style scoped>
.dp-rl-wrap {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
  width: 100%;
  margin-top: 130px;
}

.panel {
  border: 1px solid rgba(120, 120, 120, 0.28);
  border-radius: 16px;
  padding: 9px;
  background: rgba(255, 255, 255, 0.45);
  backdrop-filter: blur(6px);
}

.panel-head {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  margin-bottom: 6px;
  padding-left: 4px;
}

.panel h2 {
  font-size: 0.92rem;
  line-height: 1.1;
  font-weight: 800;
  margin-top: 2px;
  margin-bottom: 15px;
}

.grid-area {
  display: flex;
  justify-content: center;
}

.grid-wrap {
  position: relative;
  width: 231px;
  height: 231px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(6, 36px);
  grid-template-rows: repeat(6, 36px);
  gap: 3px;
}

.arrow-layer {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 3;
}

.arrow {
  stroke: rgba(160, 120, 255, 0.72);
  stroke-width: 3;
  stroke-linecap: round;
}

.cell {
  position: relative;
  border-radius: 8px;
  border: 1px solid rgba(120, 120, 120, 0.32);
  background: rgba(255, 255, 255, 0.55);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.22s ease, background 0.22s ease, border-color 0.22s ease, box-shadow 0.22s ease;
}

.cell.path {
  background: rgba(255, 75, 75, 0.18);
  border-color: rgba(255, 75, 75, 0.55);
}

.cell.focus {
  border-color: rgba(255, 150, 40, 0.95);
  box-shadow: inset 0 0 0 2px rgba(255, 150, 40, 0.65), 0 0 0 3px rgba(255, 150, 40, 0.14);
  z-index: 2;
}

.cell.path.focus {
  background: rgba(255, 75, 75, 0.18);
  border-color: rgba(255, 150, 40, 0.95);
  box-shadow: inset 0 0 0 2px rgba(255, 150, 40, 0.7), 0 0 0 3px rgba(255, 150, 40, 0.16);
  z-index: 2;
}

.cell.active {
  transform: scale(1.08);
  background: rgba(255, 75, 75, 0.34);
  border-color: rgba(255, 75, 75, 0.95);
  box-shadow: 0 0 0 3px rgba(255, 75, 75, 0.18);
  z-index: 2;
}

.cell.stable {
  background: rgba(140, 140, 140, 0.18);
  border-color: rgba(140, 140, 140, 0.55);
}

.cell.sweep {
  box-shadow: inset 0 0 0 1px rgba(120, 160, 255, 0.18);
}

.cell.start {
  border-color: rgba(60, 150, 255, 0.95);
  box-shadow: inset 0 0 0 2px rgba(60, 150, 255, 0.55);
}

.cell.goal {
  border-color: rgba(60, 180, 100, 0.95);
  box-shadow: inset 0 0 0 2px rgba(60, 180, 100, 0.55);
}

.value {
  font-size: 0.82rem;
  font-weight: 800;
  line-height: 1;
}

.value.label {
  font-size: 0.47rem;
  font-weight: 900;
  letter-spacing: -0.02em;
}

.controls {
  display: flex;
  justify-content: center;
  gap: 6px;
  margin-top: 6px;
}

button {
  border: 1px solid rgba(120, 120, 120, 0.4);
  border-radius: 9px;
  padding: 3px 8px;
  font-size: 0.68rem;
  background: rgba(255, 255, 255, 0.65);
  cursor: pointer;
}

button:hover {
  background: rgba(255, 255, 255, 0.92);
}

:global(.dark) .panel {
  background: rgba(20, 20, 24, 0.42);
}

:global(.dark) .cell {
  background: rgba(30, 30, 36, 0.72);
}

:global(.dark) .cell.path {
  background: rgba(255, 75, 75, 0.2);
}

:global(.dark) .cell.path.focus {
  background: rgba(255, 75, 75, 0.2);
  border-color: rgba(255, 165, 55, 0.95);
  box-shadow: inset 0 0 0 2px rgba(255, 165, 55, 0.72), 0 0 0 3px rgba(255, 165, 55, 0.16);
}

:global(.dark) .cell.stable {
  background: rgba(160, 160, 160, 0.16);
  border-color: rgba(180, 180, 180, 0.42);
}

:global(.dark) button {
  background: rgba(30, 30, 36, 0.72);
  color: white;
}
</style>