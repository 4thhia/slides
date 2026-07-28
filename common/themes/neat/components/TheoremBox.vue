<script setup lang="ts">
withDefaults(
  defineProps<{
    label?: string
    number?: string | number
    name?: string
  }>(),
  {
    label: 'Theorem',
    number: '',
    name: '',
  },
)
</script>

<template>
  <section class="theorem-box">
    <div class="theorem-head">
      <span class="theorem-label">
        {{ label }}<span v-if="number">&nbsp;{{ number }}</span>.
      </span>

      <span v-if="name" class="theorem-name">
        {{ name }}
      </span>
    </div>

    <div class="theorem-body">
      <slot />
    </div>
  </section>
</template>