<script setup lang="ts">
import { computed } from 'vue'
import Terminal from './Terminal.vue'

// The iceberg from slides 4 to 6, built on the original SVG so it stays sharp and editable.
//   step 1: zoomed in on the tip (your code and its libraries)
//   step 2: morph out from step 1, then reveal the runtime; Linux Libraries and Linux Kernel arrive on clicks 1 and 2
//   step 3: slide Linux Libraries down to reveal ???, plus the Dockerfile line and the brace
const props = withDefaults(defineProps<{ step?: 1 | 2 | 3; click?: number }>(), { step: 1, click: 0 })
const base = import.meta.env.BASE_URL
const initialMorph = computed(() => props.step === 1 || (props.step === 2 && props.click === 0))

// Positions are percentages of the 16:9 stage.
const zoomed = computed(() => props.step === 1)
const stageStyle = computed(() => ({
  transformOrigin: '50.6% 6.3%',
  transform: zoomed.value ? 'scale(2.8)' : 'scale(1)',
  viewTransitionName: initialMorph.value ? 'iceberg-art' : 'none',
}))
const tip = computed(() =>
  zoomed.value
    ? [
        { text: 'Your application code', x: 48, y: 59, name: 'iceberg-code' },
        { text: 'Application Libraries', x: 48, y: 83, name: 'iceberg-app-libs' },
      ]
    : [
        { text: 'Your application code', x: 50, y: 25, name: 'iceberg-code' },
        { text: 'Application Libraries', x: 50, y: 37, name: 'iceberg-app-libs' },
      ],
)
const layers = computed(() => {
  const full = props.step === 3
  return [
    { text: 'Language Runtime', y: 55.5, kind: 'runtime', show: props.step >= 2 },
    { text: '???', y: 65, kind: 'unknown', show: full },
    { text: 'Linux Libraries', y: full ? 74.7 : 70, kind: 'libs', show: full || (props.step === 2 && props.click >= 1) },
    { text: 'Linux Kernel', y: 84, kind: 'kernel', show: full || (props.step === 2 && props.click >= 2) },
  ]
})
const layerTransitionName = (kind: string) => {
  if (props.step === 2 && props.click === 0 && kind === 'runtime') return 'iceberg-runtime'
  if (props.step === 2 && props.click >= 2 && kind === 'libs') return 'iceberg-libs'
  if (props.step === 3 && (kind === 'unknown' || kind === 'libs')) return `iceberg-${kind}`
  return 'none'
}
</script>

<template>
  <div class="iceberg">
    <div class="stage" :style="stageStyle">
      <img class="art" :src="`${base}img/iceberg.svg`" alt="" />
    </div>

    <span v-for="l in tip" :key="l.text" class="pill app" :class="{ big: zoomed }" :style="{ left: l.x + '%', top: l.y + '%', viewTransitionName: initialMorph ? l.name : 'none' }">{{ l.text }}</span>

    <span v-for="l in layers" :key="l.text" class="pill fade" :class="[l.kind, { shown: l.show }]" :style="{ left: '50%', top: l.y + '%', viewTransitionName: l.show ? layerTransitionName(l.kind) : 'none' }">{{ l.text }}</span>

    <template v-if="step === 3">
      <div class="brace" aria-hidden="true">{</div>
      <Terminal class="dockerfile" title="Dockerfile" icon="🐳" :lines="['FROM node:latest']" style="--term-s: 1.1; --term-size: 1.2rem" />
    </template>
  </div>
</template>

<style scoped>
.iceberg { position: absolute; inset: 0; overflow: hidden; background: #93cfe1; }
.stage { position: absolute; inset: 0; }
.art { width: 100%; height: 100%; display: block; }
.pill {
  position: absolute; transform: translate(-50%, -50%); white-space: nowrap;
  padding: 0.45rem 2.2rem; border-radius: 999px; font-weight: 700; font-size: 1.45rem; line-height: 1.35; color: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.25);
}
.pill.app { background: rgba(48, 186, 120, 0.9); color: var(--pine); font-size: 0.7rem; padding: 0.15rem 0.9rem; }
.pill.app.big { font-size: 1.45rem; padding: 0.45rem 2.2rem; }
.fade { opacity: 0; transition: opacity 0.6s ease, top 0.6s ease; }
.fade.shown { opacity: 1; }
.runtime { background: var(--blue); }
.unknown { background: var(--persimmon); color: var(--pine); }
.libs { background: #0a3fbf; }
.kernel { background: #1a1f6b; }
.brace { position: absolute; left: 28.5%; top: 47.5%; font-size: 17rem; font-weight: 300; line-height: 1; color: #c9d3ff; transform: scaleX(1.6); transform-origin: left top; }
.dockerfile { position: absolute; left: 1.8%; top: 62%; width: 27%; }
</style>
