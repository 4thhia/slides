<template>
  <div class="slidev-layout default">
    <div class="header" v-if="headerEnable">
      <p>{{ headerTitle }}</p>
      <img :src="headerLogo" alt="logo" v-if="headerLogo" />
    </div>

    <div class="content">
      <slot />
    </div>

    <div class="page-number" v-if="pageNumber">
      {{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  headerEnable: Boolean,
  headerTitle: String,
  headerLogo: String,
  pageNumber: Boolean,
});
</script>

<style>
.slidev-layout.default {
  @apply h-full;

  position: relative;

  h1 {
    @apply text-2xl;
    line-height: 1;
  }

  h2 {
    @apply text-xl;
    line-height: 1;
  }

  h3 {
    @apply text-lg;
    line-height: 1;
  }

  h4 {
    @apply text-base;
    line-height: 1;
  }

  h5 {
    @apply text-sm;
    line-height: 1;
  }

  .header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    height: 24px;
    margin-bottom: 1.2rem;
    border-bottom: 0.5px solid var(--slidev-theme-light-divider);

    p {
      display: block;
      margin: 0;
      padding: 0;
      font-size: 0.7rem;
      font-weight: bold;
    }

    img {
      display: block;
      height: 1em;
    }
  }

  .content {
    @apply h-full;
  }

  .page-number {
    position: absolute;
    right: 24px;
    bottom: 16px;
    font-size: 0.68rem;
    line-height: 1;
    opacity: 0.55;
    color: var(--slidev-theme-light-text);
    z-index: 50;
    pointer-events: none;
  }
}

html.dark {
  .slidev-layout.default {
    .header {
      border-bottom: 0.5px solid var(--slidev-theme-dark-divider);
    }

    .page-number {
      color: var(--slidev-theme-dark-text);
    }
  }
}
</style>