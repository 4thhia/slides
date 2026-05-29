<template>
  <div class="slidev-layout cover">
    <div class="background" ref="backgroundRef">
      <div class="circle circle-1" :style="circleStyle(0)" />
      <div class="circle circle-2" :style="circleStyle(1)" />
      <div class="circle circle-3" :style="circleStyle(2)" />
    </div>

    <div class="container">
      <img class="logo" :src="coverLogo" alt="cover logo" v-if="coverLogo" />
      <h1 class="title">{{ coverTitle }}</h1>
      <h2 class="author">{{ coverAuthor }}</h2>

      <div class="cover-extra" v-if="coverCollaborator">
        <span class="cover-extra-label">Collaborator:</span>
        {{ coverCollaborator }}
      </div>

      <div class="cover-extra" v-if="coverSupervisor">
        <span class="cover-extra-label">Supervisor:</span>
        {{ coverSupervisor }}
      </div>

      <div class="description">
        <slot />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const props = defineProps({
  coverTitle: String,
  coverAuthor: String,
  coverLogo: String,
  coverCollaborator: String,
  coverSupervisor: String,
});

const backgroundRef = ref(null);

const initialCircles = [
  {
    x: 460,
    y: 120,
    vx: -0.025,
    vy: -0.02,
    width: 600,
    height: 600,
  },
  {
    x: 520,
    y: -300,
    vx: -0.015,
    vy: 0.02,
    width: 700,
    height: 700,
  },
  {
    x: 250,
    y: 280,
    vx: 0.01,
    vy: -0.02,
    width: 800,
    height: 800,
  },
];

const circles = ref(initialCircles.map((c) => ({ ...c })));

let animationId;
let lastTime = null;

const REPULSION_DISTANCE = 200;
const REPULSION_STRENGTH = 0.000015;
const MIN_SPEED = 0.012;
const MAX_SPEED = 0.025;

const circleStyle = (index) => {
  const c = circles.value[index];

  return {
    width: `${c.width}px`,
    height: `${c.height}px`,
    transform: `translate(${c.x}px, ${c.y}px)`,
  };
};

const normalizeSpeed = (vx, vy) => {
  const speed = Math.sqrt(vx * vx + vy * vy);

  if (speed === 0) {
    const angle = Math.random() * Math.PI * 2;

    return {
      vx: Math.cos(angle) * MIN_SPEED,
      vy: Math.sin(angle) * MIN_SPEED,
    };
  }

  if (speed < MIN_SPEED) {
    return {
      vx: (vx / speed) * MIN_SPEED,
      vy: (vy / speed) * MIN_SPEED,
    };
  }

  if (speed > MAX_SPEED) {
    return {
      vx: (vx / speed) * MAX_SPEED,
      vy: (vy / speed) * MAX_SPEED,
    };
  }

  return { vx, vy };
};

const animate = (time) => {
  if (lastTime === null) {
    lastTime = time;
  }

  const dt = Math.min(time - lastTime, 32);
  lastTime = time;

  const background = backgroundRef.value;

  if (!background) {
    lastTime = null;
    animationId = requestAnimationFrame(animate);
    return;
  }

  const bounds = background.getBoundingClientRect();
  const W = bounds.width;
  const H = bounds.height;

  if (W <= 0 || H <= 0) {
    lastTime = null;
    animationId = requestAnimationFrame(animate);
    return;
  }

  // まず通常の移動を反映
  const nextCircles = circles.value.map((c) => ({
    ...c,
    x: c.x + c.vx * dt,
    y: c.y + c.vy * dt,
  }));

  // 近づきすぎた円同士を少し反発させる
  for (let i = 0; i < nextCircles.length; i++) {
    for (let j = i + 1; j < nextCircles.length; j++) {
      const a = nextCircles[i];
      const b = nextCircles[j];

      const ax = a.x + a.width / 2;
      const ay = a.y + a.height / 2;
      const bx = b.x + b.width / 2;
      const by = b.y + b.height / 2;

      const dx = ax - bx;
      const dy = ay - by;
      const dist = Math.sqrt(dx * dx + dy * dy) || 1;

      if (dist < REPULSION_DISTANCE) {
        const ux = dx / dist;
        const uy = dy / dist;
        const force = (1 - dist / REPULSION_DISTANCE) * REPULSION_STRENGTH * dt;

        a.vx += ux * force;
        a.vy += uy * force;
        b.vx -= ux * force;
        b.vy -= uy * force;
      }
    }
  }

  // 壁反射 + 速度下限 + 速度上限
  circles.value = nextCircles.map((c) => {
    let x = c.x;
    let y = c.y;
    let vx = c.vx;
    let vy = c.vy;

    const minX = -c.width * 0.6;
    const maxX = W - c.width * 0.6;
    const minY = -c.height * 0.4;
    const maxY = H - c.height * 0.4;

    if (x <= minX || x >= maxX) {
      vx = -vx;
      x = Math.max(minX, Math.min(x, maxX));
    }

    if (y <= minY || y >= maxY) {
      vy = -vy;
      y = Math.max(minY, Math.min(y, maxY));
    }

    const normalized = normalizeSpeed(vx, vy);
    vx = normalized.vx;
    vy = normalized.vy;

    return {
      ...c,
      x,
      y,
      vx,
      vy,
    };
  });

  animationId = requestAnimationFrame(animate);
};

onMounted(() => {
  animationId = requestAnimationFrame(animate);
});

onUnmounted(() => {
  if (animationId) {
    cancelAnimationFrame(animationId);
  }
});
</script>

<style>
.slidev-layout.cover {
  .background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 0;
    filter: blur(3px);
    overflow: hidden;
  }

  .circle {
    position: absolute;
    top: 0;
    left: 0;
    border-radius: 50%;
    will-change: transform;
  }

  .circle-1 {
    background-color: #A5D7A7;
    opacity: 0.11;
  }

  .circle-2 {
    background-color: #7FBBFF;
    opacity: 0.15;
  }

  .circle-3 {
    background-color: #F9A19A;
    opacity: 0.2;
  }

  .container {
    position: relative;
    margin-top: 100px;
    z-index: 1;
  }

  img.logo {
    width: auto;
    height: 30px;
    display: block;
    margin-bottom: 8px;
  }

  h1.title {
    font-size: 2.5rem;
    line-height: 1;
    margin-bottom: 0;
    white-space: pre-line;
  }

  h2.author {
    font-size: 1.35rem;
    line-height: 1.25;
    font-weight: 900;
    margin-top: 1.2rem;
  }

  .cover-extra {
    font-size: 1.05rem;
    line-height: 1.25;
    font-weight: normal;
    margin-top: 0.55rem;
  }

  .cover-extra-label {
    font-weight: bold;
  }

  hr {
    width: 160px;
    margin-top: 12px;
    margin-bottom: 24px;
    height: 1px;
    background-color: black !important;
  }

  .description {
    margin-top: 40px;
    font-size: 1rem;
    line-height: 1.5;
  }
}

.dark {
  .slidev-layout.cover {
    .background {
      mix-blend-mode: luminosity;
    }

    hr {
      background-color: white !important;
    }
  }
}

@media print {
  .slidev-layout.cover .circle-1,
  .slidev-layout.cover .circle-2,
  .slidev-layout.cover .circle-3 {
    animation: none !important;
  }
}
</style>