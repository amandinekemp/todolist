<template>
  <form @submit.prevent="addTask">
    <fieldset role="group">
      <input
          v-model="newTask"
          type="text"
          placeholder="Tâche à effectuer"
      />
      <button :disabled="newTask.length === 0">Ajouter</button>
    </fieldset>
  </form>

  <div v-if="tasks.length === 0">
    Vous n'avez pas de tâche à faire
  </div>

  <div v-else>
    <ul>
      <li
          v-for="task in sortedTasks()"
          :key="task.date"
          :class="{ completed: task.completed }"
      >
        <label>
          <input type="checkbox" v-model="task.completed" />
          {{ task.title }}
        </label>
      </li>
    </ul>

    <label>
      <input type="checkbox" v-model="hideCompleted" />
      Masquer les tâches complétées
    </label>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const newTask = ref('')
const hideCompleted = ref(false)
const tasks = ref([
  {
    title: 'Tâche de test',
    completed: true,
    date: 1,
  },
  {
    title: 'Tâche 2',
    completed: false,
    date: 2,
  },
])

const addTask = () => {
  tasks.value.push({
    title: newTask.value,
    completed: false,
    date: Date.now(),
  })

  newTask.value = ''
}

const sortedTasks = () => {
  const sortedTasks = tasks.value
      .slice()
      .sort((a, b) => (a.completed > b.completed ? 1 : -1))

  if (hideCompleted.value === true) {
    return sortedTasks.filter(task => !task.completed)
  }

  return sortedTasks
}
</script>

<style>
.completed {
  opacity: 0.5;
  text-decoration: line-through;
}
</style>
