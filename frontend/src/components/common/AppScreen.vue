<script setup lang="ts">
defineProps<{
  /** hide the standard top padding, e.g. when a header slot handles its own spacing */
  noTopPadding?: boolean
}>()
</script>

<template>
  <div class="app-screen">
    <header v-if="$slots.header" class="app-screen__header">
      <slot name="header" />
    </header>

    <main class="app-screen__content" :class="{ 'app-screen__content--flush-top': noTopPadding }">
      <slot />
    </main>

    <footer v-if="$slots.footer" class="app-screen__footer">
      <slot name="footer" />
    </footer>
  </div>
</template>

<style scoped>
/* Shared shell for every screen: fixed left/right margin, a scrollable
   middle section, and an optional sticky action bar pinned above the
   safe area — the structure every one of the 12 mockup screens reuses. */
.app-screen {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  max-width: var(--screen-max-width);
  margin-inline: auto;
  background: var(--color-bg);
}

.app-screen__header {
  flex-shrink: 0;
  padding: var(--space-5) var(--screen-padding-x) var(--space-3);
}

.app-screen__content {
  flex: 1;
  padding: var(--space-6) var(--screen-padding-x);
  display: flex;
  flex-direction: column;
  gap: var(--section-gap);
}

.app-screen__content--flush-top {
  padding-top: 0;
}

.app-screen__footer {
  flex-shrink: 0;
  position: sticky;
  bottom: 0;
  padding: var(--cta-bar-padding-top) var(--screen-padding-x) var(--cta-bar-padding-bottom);
  background: linear-gradient(to top, var(--color-bg) 65%, transparent);
}
</style>
