<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>
  <div class="app-shell">
    <header class="site-header">
      <div class="brand">Personal Site</div>
      <nav class="nav-links">
        <RouterLink class="nav-link" to="/">Home</RouterLink>
        <RouterLink class="nav-link" to="/messages">Messages</RouterLink>
      </nav>
    </header>

    <main class="site-main">
      <RouterView />
    </main>
  </div>
</template>

<style scoped>
.app-shell {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.site-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 16px 0;
  border-bottom: 1px solid var(--color-border);
}

.brand {
  font-size: 1.125rem;
  font-weight: 600;
}

.nav-links {
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
}

.nav-link {
  padding: 6px 12px;
  border-radius: 8px;
  border: 1px solid var(--color-border);
  color: inherit;
  text-decoration: none;
  transition: border-color 0.2s ease, background-color 0.2s ease;
}

.nav-link.router-link-exact-active {
  background-color: var(--color-background-soft);
  border-color: var(--color-border-hover);
}

.nav-link:hover {
  border-color: var(--color-border-hover);
}

.site-main {
  flex: 1;
  padding-top: 24px;
}
</style>
