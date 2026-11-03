<script setup lang="ts">
import { computed } from 'vue'

// A window like the terminal screenshots in the Keynote deck: three dots, a small grey title and
// the code on a near-black body. Pass `lines` for short snippets (rendered with line numbers and a
// little highlighting), `yaml` for the light policy window, or put your own markup in the slot.
const props = withDefaults(
  defineProps<{ title?: string; icon?: string; lines?: string[]; numbers?: boolean; light?: boolean; yaml?: boolean }>(),
  { title: 'Terminal', numbers: true },
)

const esc = (s: string) => s.replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;')

function shell(l: string) {
  return esc(l)
    .replace(/^(&gt;|❯)/, '<span class="t-prompt">$1</span>')
    .replace(/\b(FROM)\b/, '<span class="t-kw">$1</span>')
    .replace(/(?<=:)(\d+)/, '<span class="t-num">$1</span>')
}

function yaml(l: string) {
  const e = esc(l)
  const cm = e.match(/^(.*?)(\s#.*)$/)
  const body = cm ? cm[1] : e
  const comment = cm ? `<span class="y-c">${cm[2]}</span>` : ''
  const m = body.match(/^(\s*(?:-\s)?)([A-Za-z]\w*)(:)(.*)$/)
  if (m) {
    const rest = /^\s*true$/.test(m[4]) ? `<span class="y-b">${m[4]}</span>` : `<span class="y-v">${m[4]}</span>`
    return `${m[1]}<span class="y-k">${m[2]}${m[3]}</span>${rest}${comment}`
  }
  if (/^\s*(-----|[A-Za-z0-9+/=]+$|- \|)/.test(body)) return `<span class="y-k">${body}</span>${comment}`
  return body + comment
}

const html = computed(() => (props.lines ?? []).map((l) => (props.yaml ? yaml(l) : shell(l))))
</script>

<template>
  <div class="term" :class="{ light }">
    <div class="term-bar"><i /><i /><i /><span><em v-if="icon">{{ icon }}</em>{{ title }}</span></div>
    <div class="term-body">
      <div v-if="lines" class="term-lines">
        <div v-for="(l, i) in html" :key="i" class="term-line">
          <b v-if="numbers">{{ i + 1 }}</b><span v-html="l" />
        </div>
      </div>
      <slot />
    </div>
  </div>
</template>

<style scoped>
.term { background: #12171d; border-radius: 10px; overflow: hidden; color: #e6edf3; box-shadow: 0 10px 28px rgba(0, 0, 0, 0.45); text-align: left; }
.term { --s: var(--term-s, 1); }
.term-bar { position: relative; display: flex; gap: calc(6px * var(--s)); align-items: center; padding: var(--term-bp, calc(0.55rem * var(--s))) calc(0.9rem * var(--s)); background: #181e26; }
.term-bar i { width: calc(0.6rem * var(--s)); height: calc(0.6rem * var(--s)); border-radius: 50%; background: #ff5f56; }
.term-bar i:nth-child(2) { background: #ffbd2e; }
.term-bar i:nth-child(3) { background: #27c93f; }
.term-bar span { position: absolute; left: 0; right: 0; text-align: center; font: calc(0.7rem * var(--s)) "JetBrains Mono", ui-monospace, monospace; color: #7d8590; pointer-events: none; }
.term-bar em { font-style: normal; margin-right: 0.5em; }
.term-body { padding: var(--term-pad, 0.7rem 1.1rem 0.9rem); font-family: "JetBrains Mono", ui-monospace, monospace; font-size: var(--term-size, 1rem); line-height: var(--term-lh, 1.5); }
.term-line { display: flex; gap: var(--term-gap, 1.1rem); white-space: pre; }
.term-line b { color: #4b5563; font-weight: 400; min-width: var(--term-nw, 2ch); text-align: right; }
:deep(.t-kw) { color: var(--persimmon); }
:deep(.t-num) { color: #58a6ff; }
:deep(.t-prompt) { color: #ff7b72; margin-right: 0.6ch; }

.term.light { background: #fff; color: #3b4048; box-shadow: 0 10px 28px rgba(0, 0, 0, 0.35); }
.term.light .term-bar { background: #fff; padding: 0.9rem 1.1rem 0.4rem; }
.term.light .term-bar i { width: 0.8rem; height: 0.8rem; }
.term.light .term-bar span { color: #24292f; font-family: inherit; font-size: 0.95rem; font-weight: 500; }
.term.light .term-line b { color: #9aa0a6; }
:deep(.y-k) { color: #2563c4; font-style: italic; }
:deep(.y-v) { color: #4a9d2f; }
:deep(.y-b) { color: #e5731b; }
:deep(.y-c) { color: #8a8f98; font-style: italic; }
</style>
