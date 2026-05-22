<template>
  <div class="mdp-demo">
    <div class="mdp-stage">
      <div class="frame-wrap" @click="nextStep">
        <img
          :src="currentFrame"
          class="frame-image"
          alt="Sequential decision making"
        />
      </div>

      <div class="controls">
        <button class="icon-button arrow-button" @click.stop="prevStep" aria-label="Previous">◁</button>
        <button class="icon-button arrow-button" @click.stop="nextStep" aria-label="Next">▷</button>
        <button class="icon-button reset-button" @click.stop="resetStep" aria-label="Reset">↺</button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const frames = [
  '/mdp/step0.png',
  '/mdp/step1.png',
  '/mdp/step2.png',
  '/mdp/step3.png',
  '/mdp/step4.png',
  '/mdp/step5.png',
  '/mdp/step6.png',
  '/mdp/step7.png',
  '/mdp/step8.png',
];

const step = ref(0);
const maxStep = frames.length - 1;

const currentFrame = computed(() => frames[step.value]);

const nextStep = () => {
  if (step.value < maxStep) {
    step.value += 1;
  }
};

const prevStep = () => {
  if (step.value > 0) {
    step.value -= 1;
  }
};

const resetStep = () => {
  step.value = 0;
};
</script>

<style scoped>
.mdp-demo {
  position: absolute;
  left: 40px;
  top: 250px;
  width: 320px;
  display: flex;
  justify-content: flex-start;
}

.mdp-stage {
  width: 320px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.frame-wrap {
  width: 100%;
  display: flex;
  justify-content: center;
  cursor: pointer;
}

.frame-image {
  width: 100%;
  height: auto;
  display: block;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 6px;
  margin-top: 10px;
  width: 100%;
}

.icon-button {
  width: 22px;
  height: 22px;
  border: none;
  border-radius: 999px;
  padding: 0;
  font-size: 0.72rem;
  line-height: 1;
  background: transparent;
  color: rgba(0, 0, 0, 0.6);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-button:hover {
  background: rgba(0, 0, 0, 0.06);
  color: rgba(0, 0, 0, 0.72);
}

.arrow-button {
  font-weight: 200;
  font-size: 0.82rem;
  text-shadow: 0.35px 0 0 currentColor, -0.35px 0 0 currentColor;
}

.reset-button {
  font-size: 0.78rem;
}

:global(.dark) .icon-button {
  color: rgba(255, 255, 255, 0.48);
}

:global(.dark) .icon-button:hover {
  background: rgba(255, 255, 255, 0.08);
  color: rgba(255, 255, 255, 0.78);
}
</style>