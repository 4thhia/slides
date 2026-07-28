<template>
  <button
    type="button"
    class="icon-btn remembered-slide-button"
    title="Go to previous slide and restore its click state"
    aria-label="Go to previous slide and restore its click state"
    :disabled="currentPage <= 1"
    @click="moveSlide(-1)"
  >
    <span
      class="remembered-slide-icon"
      aria-hidden="true"
    >
      ↑
    </span>
  </button>

  <button
    type="button"
    class="icon-btn remembered-slide-button"
    title="Go to next slide and restore its click state"
    aria-label="Go to next slide and restore its click state"
    :disabled="currentPage >= total"
    @click="moveSlide(1)"
  >
    <span
      class="remembered-slide-icon"
      aria-hidden="true"
    >
      ↓
    </span>
  </button>
</template>

<script setup lang="ts">
import { useNav } from '@slidev/client'
import { watch } from 'vue'

const {
  currentPage,
  clicks,
  total,
  go,
} = useNav()

const clickMemory = new Map<number, number>()

watch(
  [currentPage, clicks],
  ([page, click]) => {
    clickMemory.set(page, click)
  },
  { immediate: true },
)

async function moveSlide(offset: -1 | 1) {
  const current = currentPage.value
  const target = current + offset

  if (target < 1 || target > total.value)
    return

  clickMemory.set(current, clicks.value)

  const targetClicks = clickMemory.get(target) ?? 0

  await go(target, targetClicks)
}
</script>

<style scoped>
.remembered-slide-button {
  width: 2.25rem;
  height: 2.25rem;
  min-width: 2.25rem;

  display: inline-flex;
  align-items: center;
  justify-content: center;

  margin: 0;
  padding: 0;

  border-radius: 0.375rem;

  color: #585858;
  font-size: 1.1rem;
  line-height: 1;

  transition:
    background-color 150ms ease,
    color 150ms ease,
    opacity 150ms ease;
}

.remembered-slide-button:not(:disabled):hover {
  color: #585858;
  background-color: #f5f6f7;
}

.remembered-slide-button:disabled {
  color: rgb(136 136 136 / 35%);
  cursor: default;
}

.remembered-slide-icon {
  display: block;
  transform: translateY(-0.5px);
}
</style>