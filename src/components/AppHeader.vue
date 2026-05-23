<script setup>
// CONCEPTS: scoped styles, static component
</script>

<template>
  <header class="header">
    <span class="logo">Get Sh*t Done</span>
  </header>
</template>

<style scoped>
.header {
  background: var(--black);
  padding: 0 32px;
  height: 56px;
  display: flex;
  align-items: center;
  position: sticky;
  top: 0;
  z-index: 100;
}

.logo {
  font-family: var(--font);
  font-size: 18px;
  font-weight: 800;
  color: var(--white);
  letter-spacing: -0.03em;
}
</style>
