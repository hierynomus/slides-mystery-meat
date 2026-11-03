<script setup lang="ts">
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

// Full-bleed photo from the active image theme. `panel` puts a dark text panel on one half; `size: 'sm'` is the
// smaller bullet size the original uses on its two-line panels. `position` is the horizontal crop
// (0 to 100) when the photo is wider than 16:9. `bgzoom` (percent, over 100) scales the photo up from its
// top-left corner, which pushes the subject right and crops the bottom and right edges.
const props = defineProps<{ image?: string; panel?: 'left' | 'right' | 'none'; position?: number; bgzoom?: number; size?: 'sm' }>()
// Background photos live in public/themes/<theme>/. The theme comes from the VITE_DECK_THEME environment
// variable (set when starting the deck), else `imageTheme` in the first slide's frontmatter, else "burgers".
const { $slidev } = useSlideContext()
const theme = import.meta.env.VITE_DECK_THEME || $slidev?.configs?.imageTheme || 'burgers'
const url = computed(() =>
  props.image ? `${import.meta.env.BASE_URL}themes/${theme}/${props.image.replace(/^\//, '')}` : undefined,
)
const style = computed(() =>
  url.value
    ? {
        backgroundImage: `url(${url.value})`,
        backgroundPosition: `${props.position ?? 50}% ${props.bgzoom ? 0 : 50}%`,
        ...(props.bgzoom ? { backgroundSize: `auto ${props.bgzoom}%` } : {}),
      }
    : {},
)
</script>

<template>
  <div class="slidev-layout photo" :style="style">
    <div v-if="panel && panel !== 'none'" class="photo-panel" :class="[panel, size]"><slot /></div>
    <div v-else class="photo-free"><slot /></div>
  </div>
</template>
