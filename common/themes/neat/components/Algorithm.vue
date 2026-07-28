<script setup lang="ts">
import { computed } from 'vue'
import katex from 'katex'
import 'katex/dist/katex.min.css'

const props = withDefaults(
  defineProps<{
    label?: string
    number?: string | number
    title?: string
    compact?: boolean
    tight?: boolean
  }>(),
  {
    label: 'Algorithm',
    number: '',
    title: '',
    compact: false,
    tight: false,
  },
)

function escapeHtml(s: string) {
  return s
    .replaceAll('&', '&amp;')
    .replaceAll('<', '&lt;')
    .replaceAll('>', '&gt;')
    .replaceAll('"', '&quot;')
    .replaceAll("'", '&#039;')
}

function renderInlineMath(s: string) {
  const re = /\$([^$\n]+?)\$/g
  let out = ''
  let last = 0

  for (const match of s.matchAll(re)) {
    const index = match.index ?? 0
    const rawText = s.slice(last, index)
    const tex = match[1]

    out += escapeHtml(rawText)
    out += katex.renderToString(tex, {
      throwOnError: false,
      displayMode: false,
    })

    last = index + match[0].length
  }

  out += escapeHtml(s.slice(last))
  return out
}

const renderedTitle = computed(() => renderInlineMath(props.title))
</script>

<template>
  <section class="algo" :class="{ 'algo--compact': compact, 'algo--tight': tight }">
    <div class="algo-header">
      <span class="algo-label">
        {{ label }}<span v-if="number">&nbsp;{{ number }}</span>:
      </span>

      <span class="algo-title">
        <span v-if="title" v-html="renderedTitle" />
        <slot v-else name="title" />
      </span>
    </div>

    <div class="algo-body">
      <slot />
    </div>
  </section>
</template>

<style scoped>
.algo {
  --algo-font-size: var(--text-algorithm, 0.72rem);
  --algo-line-height: 1.20;
  --algo-line-gap: 0rem;
  --algo-outer-margin-y: 0.5rem;

  --algo-border-width: 2.7px;
  --algo-title-rule-width: 1.15px;

  --algo-title-padding-y: 0.26rem;
  --algo-title-padding-x: 0.12rem;

  --algo-body-padding-y: 0.26rem;
  --algo-body-padding-x: 0.12rem;

  --algo-number-col-width: 1.0rem;
  --algo-number-gap: 0.0rem;
  --algo-code-start: calc(var(--algo-number-col-width) + var(--algo-number-gap));

  --algo-block-indent: 0.95rem;
  --algo-guide-text-gap: 0.42rem;

  --algo-guide-width: 1px;
  --algo-guide-color: rgba(120, 120, 120, 0.42);
  --algo-guide-foot: 0.30rem;
  --algo-guide-top-gap: 0.08rem;
  --algo-guide-bottom-gap: 0.08rem;

  /* Default width is 100% */
  width: 100%;
  margin-block: var(--algo-outer-margin-y);
  border-top: var(--algo-border-width) solid currentColor;
  border-bottom: var(--algo-border-width) solid currentColor;
  font-size: var(--algo-font-size);
  line-height: var(--algo-line-height);
}

/* Modifier class to shrink the width to fit the content */
.algo--tight {
  width: fit-content;
}

.algo-header {
  padding: var(--algo-title-padding-y) var(--algo-title-padding-x);
  border-bottom: var(--algo-title-rule-width) solid currentColor;
  text-align: left;
  font-size: 1.03em;
  line-height: 1.2;
}

.algo-label {
  font-weight: 700;
}

.algo-title {
  font-weight: 400;
}

.algo-title :deep(p) {
  display: inline;
  margin: 0;
}

.algo-body {
  counter-reset: algo-line;
  padding: var(--algo-body-padding-y) var(--algo-body-padding-x);
  font-variant-numeric: tabular-nums;
}

.algo-body :deep(ul) {
  margin: 0;
  padding: 0;
  list-style: none;
}

.algo-body :deep(li) {
  position: relative;
  margin: var(--algo-line-gap) 0;
  padding-left: var(--algo-code-start);
}

.algo-body :deep(li::before) {
  counter-increment: algo-line;
  content: counter(algo-line);
  position: absolute;
  left: 0;
  width: var(--algo-number-col-width);
  text-align: left;
  color: currentColor;
}

/* critical fix: nested ul is pulled back to the global left origin */
.algo-body :deep(ul ul) {
  position: relative;
  margin-top: 0.04rem;
  margin-bottom: 0.04rem;
  margin-left: calc(-1 * var(--algo-code-start));
  padding: 0;
}

/* depth 1 */
.algo-body :deep(ul ul > li) {
  padding-left: calc(
    var(--algo-code-start)
    + var(--algo-block-indent)
    + var(--algo-guide-text-gap)
  );
}

/* depth 2 */
.algo-body :deep(ul ul ul) {
  margin-left: calc(-1 * (var(--algo-code-start) + var(--algo-block-indent) + var(--algo-guide-text-gap)));
}

.algo-body :deep(ul ul ul > li) {
  padding-left: calc(
    var(--algo-code-start)
    + 2 * var(--algo-block-indent)
    + var(--algo-guide-text-gap)
  );
}

/* depth 3 */
.algo-body :deep(ul ul ul ul) {
  margin-left: calc(-1 * (var(--algo-code-start) + 2 * var(--algo-block-indent) + var(--algo-guide-text-gap)));
}

.algo-body :deep(ul ul ul ul > li) {
  padding-left: calc(
    var(--algo-code-start)
    + 3 * var(--algo-block-indent)
    + var(--algo-guide-text-gap)
  );
}

/* L-shaped guide: depth 1 */
.algo-body :deep(ul ul::before) {
  content: "";
  position: absolute;
  left: var(--algo-code-start);
  top: var(--algo-guide-top-gap);
  bottom: var(--algo-guide-bottom-gap);
  width: var(--algo-guide-width);
  background: var(--algo-guide-color);
}

.algo-body :deep(ul ul::after) {
  content: "";
  position: absolute;
  left: var(--algo-code-start);
  bottom: var(--algo-guide-bottom-gap);
  width: var(--algo-guide-foot);
  height: var(--algo-guide-width);
  background: var(--algo-guide-color);
}

/* L-shaped guide: depth 2 */
.algo-body :deep(ul ul ul::before),
.algo-body :deep(ul ul ul::after) {
  left: calc(var(--algo-code-start) + var(--algo-block-indent));
}

/* L-shaped guide: depth 3 */
.algo-body :deep(ul ul ul ul::before),
.algo-body :deep(ul ul ul ul::after) {
  left: calc(var(--algo-code-start) + 2 * var(--algo-block-indent));
}

.algo-body :deep(strong) {
  font-weight: 700;
}

.algo-body :deep(code) {
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  font-size: 0.94em;
  background: transparent;
  padding: 0;
}

.algo-body :deep(li > p) {
  display: inline;
  margin: 0;
}

.algo--compact {
  --algo-font-size: calc(var(--text-algorithm, 0.72rem) - 0.02rem);
  --algo-line-height: 1.16;
  --algo-line-gap: 0rem;
  --algo-title-padding-y: 0.20rem;
  --algo-body-padding-y: 0.20rem;
  --algo-number-col-width: 1.25rem;
  --algo-number-gap: 0.58rem;
  --algo-block-indent: 0.78rem;
  --algo-guide-text-gap: 0.34rem;
}
</style>