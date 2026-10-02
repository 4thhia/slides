<template>
  <svg
    class="common-arrow"
    :viewBox="`0 0 ${canvasWidth} ${canvasHeight}`"
    preserveAspectRatio="none"
  >
    <defs>
      <marker
        :id="markerId"
        markerUnits="userSpaceOnUse"
        :markerWidth="headSize"
        :markerHeight="headSize"
        :refX="headSize"
        :refY="headSize / 2"
        orient="auto-start-reverse"
      >
        <path
          :d="`
            M 0 0
            L ${headSize} ${headSize / 2}
            L 0 ${headSize}
            Z
          `"
          :fill="color"
          :fill-opacity="opacity"
        />
      </marker>
    </defs>

    <polyline
      :points="svgPoints"
      fill="none"
      :stroke="color"
      :stroke-width="thickness"
      :stroke-opacity="opacity"
      :stroke-linecap="linecap"
      :stroke-linejoin="linejoin"
      :stroke-dasharray="dashed ? dashArray : undefined"
      :marker-start="hasStartHead ? `url(#${markerId})` : undefined"
      :marker-end="hasEndHead ? `url(#${markerId})` : undefined"
    />
  </svg>
</template>

<script setup>
import { computed, getCurrentInstance } from 'vue'

const props = defineProps({
  /**
   * Sequence of coordinates:
   *
   * [
   *   [startX, startY],
   *   [waypoint1X, waypoint1Y],
   *   [waypoint2X, waypoint2Y],
   *   ...
   *   [endX, endY],
   * ]
   *
   * The first point is the start point,
   * the last point is the end point,
   * and all points in between are optional waypoints.
   */
  points: {
    type: Array,
    required: true,
    validator: (value) =>
      Array.isArray(value) &&
      value.length >= 2 &&
      value.every(
        (p) =>
          Array.isArray(p) &&
          p.length === 2 &&
          typeof p[0] === 'number' &&
          typeof p[1] === 'number'
      ),
  },

  /**
   * Stroke width.
   */
  thickness: {
    type: Number,
    default: 2,
  },

  /**
   * Arrowhead size.
   */
  headSize: {
    type: Number,
    default: 12,
  },

  /**
   * Stroke and arrowhead color.
   */
  color: {
    type: String,
    default: '#222448',
  },

  /**
   * Opacity in the range [0, 1].
   */
  opacity: {
    type: Number,
    default: 1,
    validator: (value) => value >= 0 && value <= 1,
  },

  /**
   * Whether to use a dashed stroke.
   */
  dashed: {
    type: Boolean,
    default: false,
  },

  /**
   * SVG stroke-dasharray value used when dashed=true.
   */
  dashArray: {
    type: String,
    default: '8 6',
  },

  /**
   * Arrowhead placement:
   *
   * end   : arrowhead at the end point
   * start : arrowhead at the start point
   * both  : arrowheads at both ends
   * none  : no arrowheads
   */
  head: {
    type: String,
    default: 'end',
    validator: (value) =>
      ['end', 'start', 'both', 'none'].includes(value),
  },

  /**
   * Slide canvas width.
   */
  canvasWidth: {
    type: Number,
    default: 980,
  },

  /**
   * Slide canvas height.
   */
  canvasHeight: {
    type: Number,
    default: 552,
  },

  /**
   * SVG stroke-linecap value.
   */
  linecap: {
    type: String,
    default: 'round',
  },

  /**
   * SVG stroke-linejoin value.
   */
  linejoin: {
    type: String,
    default: 'round',
  },
})

const instance = getCurrentInstance()

const markerId = `common-arrow-${instance?.uid ?? 0}`

const svgPoints = computed(() =>
  props.points
    .map(([x, y]) => `${x},${y}`)
    .join(' ')
)

const hasStartHead = computed(() =>
  props.head === 'start' || props.head === 'both'
)

const hasEndHead = computed(() =>
  props.head === 'end' || props.head === 'both'
)
</script>

<style scoped>
.common-arrow {
  position: absolute;
  inset: 0;

  width: 100%;
  height: 100%;

  overflow: visible;

  pointer-events: none;

  z-index: 10;
}
</style>