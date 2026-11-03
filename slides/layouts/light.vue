<template>
  <div class="slidev-layout light">
    <div class="pad"><slot /></div>
    <div class="logo"><img :src="`${base}img/suse-chameleon.png`" alt="" /><span>SUSE</span></div>
  </div>
</template>

<script setup lang="ts">
const base = import.meta.env.BASE_URL
</script>

<style scoped>
.pad { padding: 2.5rem 3.6rem 5rem; height: 100%; box-sizing: border-box; position: relative; }
.logo { position: absolute; left: 2.2rem; bottom: 1.45rem; display: flex; align-items: center; gap: 0.6rem; color: var(--ink); }
.logo img { height: 1.9rem; }
.logo span { font-weight: 500; font-size: 1.9rem; letter-spacing: 0.08em; }
</style>
