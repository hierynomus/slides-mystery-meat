<template>
  <div class="slidev-layout statement">
    <div class="logo"><img :src="`${base}img/suse-chameleon.png`" alt="" /><span>SUSE</span></div>
    <div class="body"><slot /></div>
  </div>
</template>

<script setup lang="ts">
const base = import.meta.env.BASE_URL
</script>

<style scoped>
.logo { position: absolute; left: 7%; top: 17%; transform: translateY(-50%); display: flex; align-items: center; gap: 1rem; color: #fff; }
.logo img { height: 3.8rem; }
.logo span { font-weight: 400; font-size: 3.8rem; letter-spacing: 0.08em; line-height: 1; }
.body { position: absolute; inset: 0; padding: 9% 0 0 6.5%; display: flex; align-items: center; }
</style>
