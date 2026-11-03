<script setup lang="ts">
import { computed } from 'vue'
import { useNav } from '@slidev/client'

// Chrome from the Keynote deck. The four-colour bar and slide number sit on the flat layouts only;
// the `statement` layout gets the stacked-bar motif on the right instead.
const { currentLayout, currentPage, currentSlideRoute } = useNav()
// `bar: short` in a slide's frontmatter starts the bar at the blue part, where the speaker photo ends
const short = computed(() => currentLayout.value === 'intro' || currentSlideRoute.value?.meta?.slide?.frontmatter?.bar === 'short')
const bar = computed(() => ['jungle', 'light', 'intro'].includes(currentLayout.value))
const motif = computed(() => currentLayout.value === 'statement')
const dark = computed(() => ['jungle', 'statement'].includes(currentLayout.value))

// Bars of the motif as [x, y, width, colour], in the 627x353 coordinates of the Keynote slide.
const bars: [number, number, number, string][] = [
  [585, 147, 42, 'green'], [510, 166, 117, 'green'], [531, 183, 74, 'blue'], [605, 183, 22, 'orange'],
  [488, 202, 80, 'orange'], [586, 203, 41, 'mint'], [510, 223, 58, 'green'], [568, 223, 59, 'blue'],
  [585, 245, 42, 'green'],
]
const fill: Record<string, string> = { green: 'var(--jungle)', blue: 'var(--blue)', orange: 'var(--persimmon)', mint: 'var(--mint)' }
</script>

<template>
  <div v-if="bar" class="brand-bar" :class="{ short }" aria-hidden="true">
    <span style="background: var(--mint); flex: 3" />
    <span style="background: var(--blue); flex: 1.2" />
    <span style="background: var(--persimmon); flex: 0.6" />
    <span style="background: var(--jungle); flex: 3.2" />
  </div>
  <svg v-if="motif" class="motif" viewBox="0 0 627 353" preserveAspectRatio="none" aria-hidden="true">
    <rect v-for="(b, i) in bars" :key="i" :x="b[0]" :y="b[1]" :width="b[2]" height="8" :fill="fill[b[3]]" />
  </svg>
  <div v-if="bar || motif" class="num" :class="{ dark, right: motif }">{{ currentPage }}</div>
</template>

<style scoped>
.brand-bar {
  position: absolute; right: 0; bottom: 2.2rem; width: 50%; height: 0.9rem;
  display: flex; z-index: 5; pointer-events: none;
}
.brand-bar span { display: block; height: 100%; }
.brand-bar.short { width: 28%; }
.brand-bar.short span:first-child { display: none; }
.motif { position: absolute; inset: 0; width: 100%; height: 100%; z-index: 5; pointer-events: none; }
.num { position: absolute; left: 46.5%; bottom: 1.9rem; font-size: 0.85rem; font-weight: 700; z-index: 5; transform: translateX(-50%); color: var(--ink); }
.num.dark { color: #fff; }
.num.right { left: auto; right: 3.2%; transform: none; }
</style>
