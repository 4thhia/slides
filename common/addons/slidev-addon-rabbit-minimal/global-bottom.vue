<template>
  <footer
    v-if="enabled"
    class="rabbit-minimal"
    :style="appearance"
    role="img"
    :aria-label="`Slide ${current} of ${total}; ${Math.round(timeProgress * 100)} percent of allotted time elapsed`"
  >
    <div class="rabbit-minimal__track" />
    <div class="rabbit-minimal__segment" :style="segmentStyle" />
    <div class="rabbit-minimal__time" :style="{ left: `${timeProgress * 100}%` }" />
    <span v-if="options.slideNum" class="rabbit-minimal__number">{{ current }} / {{ total }}</span>
  </footer>
</template>

<script>
// Minimal visual variant of kaakaa/slidev-addon-rabbit (MIT).
export default {
  data() {
    return { elapsed: 0, startedAt: null, intervalId: null };
  },
  computed: {
    options() { return this.$slidev.configs.rabbit || {}; },
    current() { return Number(this.$slidev.nav.currentPage); },
    total() { return Number(this.$slidev.nav.total); },
    allocation() {
      const slides = this.$slidev.nav.slides || [];
      const total = this.seconds(this.options.totalDuration);
      if (total === null || total <= 0)
        return { error: 'Set rabbit.totalDuration to a positive number of seconds.', schedule: [] };
      const rows = slides.map((slide, index) => {
        const frontmatter = slide.meta?.slide?.frontmatter || {};
        const specified = Object.prototype.hasOwnProperty.call(frontmatter, 'duration');
        return {
          page: Number(slide.no ?? index + 1),
          specified,
          seconds: specified ? this.seconds(frontmatter.duration) : null,
        };
      });
      const invalid = rows.find(row => row.specified && row.seconds === null);
      if (invalid)
        return { error: `Slide ${invalid.page}: duration must be a nonnegative number of seconds.`, schedule: [] };
      const fixed = rows.reduce((sum, row) => sum + (row.seconds ?? 0), 0);
      if (fixed > total + 1e-9)
        return { error: `Specified durations (${fixed}s) exceed totalDuration (${total}s).`, schedule: [] };
      const unspecified = rows.filter(row => !row.specified).length;
      const share = unspecified ? Math.max(0, total - fixed) / unspecified : 0;
      let end = 0;
      const schedule = rows.map(row => {
        const seconds = row.specified ? row.seconds : share;
        const start = end;
        end += seconds;
        return { page: row.page, start, end, seconds };
      });
      return { error: '', schedule };
    },
    schedule() { return this.allocation.schedule; },
    duration() { return (this.seconds(this.options.totalDuration) || 0) * 1000; },
    currentSegment() { return this.schedule.find(slide => slide.page === this.current); },
    enabled() {
      // Slidev's hash router may put print in the outer URL query.
      const print = Object.prototype.hasOwnProperty.call(this.$route.query, 'print')
        || (typeof window !== 'undefined' && new URLSearchParams(window.location.search).has('print'));
      return this.options.enabled !== false && !this.allocation.error && this.schedule.length > 0 && this.duration > 0 && !print;
    },
    segmentStyle() {
      const total = this.duration / 1000;
      const segment = this.currentSegment;
      return {
        left: `${total && segment ? segment.start / total * 100 : 0}%`,
        width: `${total && segment ? segment.seconds / total * 100 : 0}%`,
        display: segment?.seconds > 0 ? 'block' : 'none',
      };
    },
    scheduleKey() { return `${this.duration}|${this.schedule.map(s => `${s.page}:${s.seconds}`).join(',')}`; },
    timeProgress() { return this.duration ? Math.min(1, this.elapsed / this.duration) : 0; },
    appearance() {
      return {
        '--rabbit-minimal-color': this.options.color || '#808080',
        opacity: this.options.opacity ?? 0.45,
        bottom: `${this.options.bottom ?? 8}px`,
        left: `${this.options.inset ?? 24}px`,
        right: `${this.options.inset ?? 24}px`,
      };
    },
  },
  watch: {
    'allocation.error': {
      immediate: true,
      handler(error) { if (error) console.warn(`[rabbit-minimal] ${error}`); },
    },
    current() { this.syncTimer(); },
    enabled() { this.syncTimer(); },
    scheduleKey() { this.resetTimer(); this.syncTimer(); },
  },
  mounted() { this.syncTimer(); },
  beforeUnmount() { this.stopTimer(); },
  methods: {
    seconds(value) {
      if (typeof value !== 'number' && typeof value !== 'string') return null;
      if (typeof value === 'string' && value.trim() === '') return null;
      const number = Number(value);
      return Number.isFinite(number) && number >= 0 ? number : null;
    },
    stopTimer() {
      if (this.intervalId !== null) clearInterval(this.intervalId);
      this.intervalId = null;
    },
    resetTimer() {
      this.stopTimer();
      this.elapsed = 0;
      this.startedAt = null;
    },
    syncTimer() {
      if (!this.enabled) { this.resetTimer(); return; }
      const first = this.schedule[0];
      if (this.current === first?.page && first.seconds === 0) { this.resetTimer(); return; }
      // A zero-duration slide before the talk starts is a waiting slide.
      // Once started, the clock continues across all slide changes.
      if (!this.currentSegment || (this.startedAt === null && this.currentSegment.seconds === 0)) return;
      if (this.startedAt !== null) return;
      this.startedAt = performance.now();
      this.intervalId = setInterval(() => {
        this.elapsed = Math.min(this.duration, performance.now() - this.startedAt);
        if (this.elapsed >= this.duration) this.stopTimer();
      }, 250);
    },
  },
};
</script>

<style scoped>
.rabbit-minimal {
  position: absolute;
  height: 10px;
  z-index: 99;
  pointer-events: none;
  user-select: none;
  color: var(--rabbit-minimal-color);
}
.rabbit-minimal__track {
  position: absolute;
  inset: 5px 0 auto;
  height: 1px;
  background: currentColor;
  opacity: 0.25;
}
.rabbit-minimal__segment {
  position: absolute;
  top: 3px;
  height: 3px;
  border-radius: 1px;
  background: currentColor;
  opacity: 0.55;
}
.rabbit-minimal__time {
  position: absolute;
  top: 1px;
  width: 1px;
  height: 9px;
  background: currentColor;
  transform: translateX(-50%);
}
.rabbit-minimal__number {
  position: absolute;
  right: 0;
  bottom: 13px;
  font: 10px/1.2 sans-serif;
  font-variant-numeric: tabular-nums;
}
@media print {
  .rabbit-minimal { display: none; }
}
</style>
