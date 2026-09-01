<script setup lang="ts">
import AppScreen from '@/components/common/AppScreen.vue'
</script>

<template>
  <AppScreen>
    <template #header>
      <p class="eyebrow">피기백</p>
    </template>

    <h1 class="title">디자인 기초 공사가<br />끝났어요</h1>
    <p class="lede">
      좌우 여백 {{ '20px' }}, 섹션 간격, 카드 라운드까지 — 12개 화면 목업에서 뽑아낸 값이
      토큰으로 정리됐고, 이 화면이 그 위에서 렌더링되고 있어요.
    </p>

    <template #footer>
      <button class="cta">확인했어요</button>
    </template>
  </AppScreen>
</template>

<style scoped>
.eyebrow {
  font-family: var(--font-mono);
  font-size: var(--text-xs);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--color-accent-deep);
}

.title {
  font-size: var(--text-2xl);
  font-weight: 700;
  line-height: 1.3;
}

.lede {
  font-size: var(--text-base);
  color: var(--color-ink-soft);
}

.cta {
  width: 100%;
  border: none;
  border-radius: var(--radius-md);
  background: var(--color-accent);
  color: var(--color-accent-ink);
  font-weight: 700;
  font-size: var(--text-base);
  padding: var(--space-4);
  cursor: pointer;
}
</style>
