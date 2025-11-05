<template>
  <article class="task-card">
    <header class="task-card__header">
      <h3 class="task-card__title">{{ safeTitle }}</h3>
      <span class="task-card__badge">{{ label }}</span>
    </header>
    <p class="task-card__text">{{ task.text }}</p>
  </article>
</template>

<script setup>
import { computed } from 'vue';

const props = defineProps({
  task: {
    type: Object,
    required: true,
  },
  label: {
    type: String,
    required: true,
  },
});

const safeTitle = computed(
  () => props.task?.title?.trim() || 'Без названия'
);
</script>

<style scoped>
.task-card {
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 16px 20px;
  border-radius: 16px;
  background: #ffffff;
  border: 1px solid rgba(15, 23, 42, 0.08);
  box-shadow: 0 10px 25px -18px rgba(15, 23, 42, 0.4);
}

.task-card__header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
  gap: 16px;
}

.task-card__title {
  margin: 0;
  font-size: 1.125rem;
  color: #0f172a;
}

.task-card__badge {
  padding: 4px 12px;
  border-radius: 999px;
  background: #e0f2fe;
  color: #0369a1;
  font-size: 0.875rem;
  font-weight: 600;
  white-space: nowrap;
}

.task-card__text {
  margin: 0;
  color: #475569;
  line-height: 1.6;
}
</style>
