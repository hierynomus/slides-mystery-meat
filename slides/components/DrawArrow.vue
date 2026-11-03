<script setup lang="ts">
// A hand-drawn style arrow: the line draws itself when `on` turns true, then the head pops in.
// Use inside an <svg>; `d` is the path of the shaft and `head` the two strokes of the arrow head.
defineProps<{ d: string; head: string; on?: boolean; color?: string; width?: number }>()
</script>

<template>
  <g :stroke="color ?? 'var(--persimmon)'" :stroke-width="width ?? 3" fill="none" stroke-linecap="round" stroke-linejoin="round">
    <path :d="d" pathLength="1" class="line" :class="{ on }" />
    <path :d="head" class="head" :class="{ on }" />
  </g>
</template>

<style scoped>
.line { stroke-dasharray: 1 1; stroke-dashoffset: 1; opacity: 0; transition: stroke-dashoffset 0.9s ease, opacity 0.05s; }
.line.on { stroke-dashoffset: 0; opacity: 1; }
.head { opacity: 0; transition: opacity 0.2s 0.8s; }
.head.on { opacity: 1; }
</style>
