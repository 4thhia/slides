<template>
  <default class="two-cols">
    <slot />

    <div class="container">
      <div
        v-if="props.verticalLine !== 'none'"
        class="vertical-divider"
        :class="props.verticalLine"
      />

      <div class="left">
        <slot name="left" />
      </div>

      <div class="right">
        <slot name="right" />
      </div>
    </div>
  </default>
</template>

<script setup>
import Default from '../layouts/default.vue'

const props = defineProps({
  verticalLine: {
    type: String,
    default: 'none',
    validator: (value) =>
      ['none', 'full', 'uhalf', 'lhalf'].includes(value),
  },
})
</script>

<style>
.slidev-layout.two-cols {
  .container {
    position: relative;
    display: flex;
    gap: 2rem;
    height: 100%;
  }

  .left {
    flex: 1;
  }

  .right {
    flex: 1;
  }

  .vertical-divider {
    position: absolute;
    left: 50%;
    width: 1px;
    background-color: var(--slidev-theme-light-divider);
    transform: translateX(-50%);
    pointer-events: none;
  }

  /* 全長 */
  .vertical-divider.full {
    top: 0;
    bottom: 1.25rem;
  }

  /* 上半分 */
  .vertical-divider.uhalf {
    top: 0;
    height: calc((100% - 1.25rem) / 2);
  }

  /* 下半分 */
  .vertical-divider.lhalf {
    bottom: 1.25rem;
    height: calc((100% - 1.25rem) / 2);
  }
}

.dark {
  .slidev-layout.two-cols {
    .vertical-divider {
      background-color: var(--slidev-theme-dark-divider);
    }
  }
}
</style>