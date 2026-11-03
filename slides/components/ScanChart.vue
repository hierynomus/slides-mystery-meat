<script setup lang="ts">
// Reported vulnerabilities (CVEs) per year. EOY = full year, H1 = first half.
// NOTE: values are read off the Keynote chart by eye; replace with the real data.
const years = [2020, 2021, 2022, 2023, 2024, 2025, 2026]
const eoy = [18000, 20000, 25000, 29000, 40000, 48000, 72000]
const h1 = [10000, 10000, 12000, 14000, 21000, 24000, 36000]

const W = 640, H = 330, left = 70, bottom = 40, top = 20
const max = 80000
const plotH = H - bottom - top
const step = (W - left - 20) / years.length
const y = (v: number) => top + plotH - (v / max) * plotH
const x = (i: number) => left + step * i + step / 2
const line = h1.map((v, i) => `${x(i)},${y(v)}`).join(' ')
const ticks = [0, 20000, 40000, 60000, 80000]
</script>

<template>
  <svg :viewBox="`0 0 ${W} ${H}`" class="chart" role="img" aria-label="Reported vulnerabilities per year, rising steeply to about 72,000 expected in 2026">
    <g v-for="t in ticks" :key="t">
      <line :x1="left" :x2="W - 10" :y1="y(t)" :y2="y(t)" stroke="#0c322c" stroke-opacity="0.25" />
      <text :x="left - 10" :y="y(t) + 5" text-anchor="end" font-size="15" fill="#0c322c">{{ t.toLocaleString('de-DE') }}</text>
    </g>
    <rect v-for="(v, i) in eoy" :key="i" :x="x(i) - 17" :y="y(v)" width="34" :height="y(0) - y(v)" rx="4" fill="#0c322c" />
    <polyline :points="line" fill="none" stroke="#30ba78" stroke-width="4" stroke-linejoin="round" stroke-linecap="round" />
    <text v-for="(yr, i) in years" :key="yr" :x="x(i)" :y="H - 12" text-anchor="middle" font-size="15" fill="#0c322c">{{ yr }}</text>
    <g class="legend" fill="#0c322c">
      <line x1="190" x2="215" y1="12" y2="12" stroke="#30ba78" stroke-width="4" /><text x="222" y="17">H1</text>
      <rect x="300" y="5" width="14" height="14" fill="#0c322c" /><text x="322" y="17">EOY</text>
    </g>
  </svg>
</template>

<style scoped>
/* Font sizes as CSS: the slide stylesheet scales bare SVG text up, so the font-size attributes lose */
.chart text { font-size: 19px; font-family: inherit; }
.chart .legend text { font-size: 19px; }
.chart { width: 100%; height: auto; background: rgba(255, 255, 255, 0.8); border-radius: 28px; border: 2px solid var(--jungle); padding: 0.6rem; box-sizing: border-box; }
</style>
