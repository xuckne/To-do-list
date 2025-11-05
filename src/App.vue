<template>
  <main class="container">
    <header class="page-header">
      <h1>Список дел</h1>
      <p>Планируйте задачи на сегодня, завтра и даже на год вперёд.</p>
    </header>

    <section class="create-task">
      <h2>Добавить задачу</h2>
      <form class="task-form" @submit.prevent="addTask">
        <div class="task-form__row">
          <label class="task-form__field">
            <span>Название</span>
            <input
              v-model.trim="form.title"
              type="text"
              placeholder="Например, купить продукты"
              required
            />
          </label>
          <label class="task-form__field">
            <span>Когда выполнить</span>
            <select v-model="form.period" required>
              <option
                v-for="option in categories"
                :key="option.value"
                :value="option.value"
              >
                {{ option.label }}
              </option>
            </select>
          </label>
        </div>

        <label class="task-form__field task-form__field--full">
          <span>Описание</span>
          <textarea
            v-model.trim="form.text"
            placeholder="Добавьте детали: важные шаги, ссылки, контакты"
            rows="3"
            required
          />
        </label>

        <button class="task-form__submit" type="submit">Добавить</button>
      </form>
    </section>

    <section class="lists">
      <article
        v-for="category in categories"
        :key="category.value"
        class="tasks-group"
      >
        <header class="tasks-group__header">
          <h2>{{ category.label }}</h2>
          <span>{{ groupCountText(category.value) }}</span>
        </header>

        <p
          v-if="groupedTasks[category.value].length === 0"
          class="tasks-group__empty"
        >
          Дел пока нет — самое время добавить первую.
        </p>

        <div class="tasks-list">
          <TaskItem
            v-for="task in groupedTasks[category.value]"
            :key="task.id"
            :task="task"
            :label="category.short"
          />
        </div>
      </article>
    </section>
  </main>
</template>

<script setup>
import { computed, reactive, ref } from 'vue';
import TaskItem from './components/TaskItem.vue';

const categories = [
  { value: 'today', label: 'Сегодня', short: 'Сегодня' },
  { value: 'tomorrow', label: 'Завтра', short: 'Завтра' },
  { value: 'yesterday', label: 'Вчера', short: 'Вчера' },
  { value: 'nextYear', label: 'Через год', short: 'Через год' },
];

const tasks = ref([
  {
    id: 1,
    title: 'Созвон с командой',
    text: 'Уточнить статус проекта и договориться о следующих шагах.',
    period: 'today',
  },
  {
    id: 2,
    title: 'Фитнес-зал',
    text: 'Записаться на функциональную тренировку во второй половине дня.',
    period: 'tomorrow',
  },
  {
    id: 3,
    title: 'Архивация файлов',
    text: 'Перенести старые документы в архив и обновить порядок.',
    period: 'yesterday',
  },
  {
    id: 4,
    title: 'Путешествие мечты',
    text: 'Выбрать направление и начать планировать отпуск через год.',
    period: 'nextYear',
  },
]);

const form = reactive({
  title: '',
  text: '',
  period: categories[0].value,
});

const groupedTasks = computed(() =>
  categories.reduce((acc, category) => {
    acc[category.value] = tasks.value.filter(
      (task) => task.period === category.value
    );
    return acc;
  }, {})
);

const groupCountText = (categoryValue) => {
  const count = groupedTasks.value[categoryValue].length;
  if (count === 0) return '0 задач';
  if (count === 1) return '1 задача';
  if (count < 5) return `${count} задачи`;
  return `${count} задач`;
};

const addTask = () => {
  const nextId =
    tasks.value.reduce((max, task) => Math.max(max, task.id), 0) + 1;

  tasks.value.push({
    id: nextId,
    title: form.title,
    text: form.text,
    period: form.period,
  });

  form.title = '';
  form.text = '';
  form.period = categories[0].value;
};
</script>
