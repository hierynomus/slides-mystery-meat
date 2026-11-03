<script setup lang="ts">
import { ref, watch, onBeforeUnmount } from 'vue'

// Types `text` out with a blinking cursor once `active` turns true (the title build on slide 13).
const props = defineProps<{ text: string; active?: boolean; speed?: number }>()
const shown = ref(0)
const typing = ref(false)
let timer: ReturnType<typeof setInterval> | undefined

watch(() => props.active, (on) => {
  clearInterval(timer)
  if (!on) { shown.value = 0; typing.value = false; return }
  typing.value = true
  timer = setInterval(() => {
    if (++shown.value >= props.text.length) { clearInterval(timer); typing.value = false }
  }, props.speed ?? 55)
}, { immediate: true })
onBeforeUnmount(() => clearInterval(timer))
</script>

<template>
  <span>{{ text.slice(0, shown) }}<span v-if="typing" class="caret">|</span></span>
</template>

<style scoped>
.caret { font-weight: 300; margin-left: 0.05em; }
</style>
