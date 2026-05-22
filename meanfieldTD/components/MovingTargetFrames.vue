<template>
  <div class="moving-target-demo">
    <div class="moving-target-stage">
      <div class="frame-wrap" @click="nextStep">
        <img
          :src="currentFrame"
          class="frame-image"
          alt="Moving target and one-step optimization"
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
  '/moving_target/step00.png',
  '/moving_target/step01.png',
  '/moving_target/step02.png',
  '/moving_target/step03.png',
  '/moving_target/step04.png',
  '/moving_target/step05.png',
  '/moving_target/step06.png',
  '/moving_target/step07.png',
  '/moving_target/step08.png',
  '/moving_target/step09.png',
  '/moving_target/step10.png',
  '/moving_target/step11.png',
  '/moving_target/step12.png',
  '/moving_target/step13.png',
  '/moving_target/step14.png',
  '/moving_target/step15.png',
  '/moving_target/step16.png',
  '/moving_target/step17.png',
  '/moving_target/step18.png',
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
.moving-target-demo {
  position: absolute;
  left: 120px;
  top: 325px;
  width: 260px;
  display: flex;
  justify-content: flex-start;
}

.moving-target-stage {
  width: 520px;
  position: relative;
}

.frame-wrap {
  position: relative;
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
  position: absolute;
  right: 8px;
  bottom: 8px;
  z-index: 20;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 6px;
}

.icon-button {
  width: 22px;
  height: 22px;
  border: none;
  border-radius: 999px;
  padding: 0;
  font-size: 0.72rem;
  line-height: 1;
  background: rgba(255, 255, 255, 0.72);
  color: rgba(0, 0, 0, 0.65);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon-button:hover {
  background: rgba(255, 255, 255, 0.9);
  color: rgba(0, 0, 0, 0.78);
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
  background: rgba(0, 0, 0, 0.35);
  color: rgba(255, 255, 255, 0.62);
}

:global(.dark) .icon-button:hover {
  background: rgba(0, 0, 0, 0.55);
  color: rgba(255, 255, 255, 0.85);
}
</style>