<template>
  <Layout>
    <template #header>
      <h1>Gestionnaire de tâches</h1>
    </template>

    <template #aside>
      <section>
        <h2>Résumé</h2>
        <ul>
          <li>📌 Total : {{ tasks.length }}</li>
          <li>✅ Complétées : {{ tasks.filter(t => t.completed).length }}</li>
          <li>🕒 Restantes : {{ remainingTasks }}</li>
        </ul>

        <hr />

        <h2>Options</h2>
        <label>
          <input type="checkbox" v-model="hideCompleted" />
          Masquer les tâches complétées
        </label>
      </section>
    </template>

    <template #main>
      <button @click="showTimer = !showTimer">Afficher / masquer</button>
      <Timer v-if="showTimer"/>
      <form action="" @submit.prevent="addTask">
        <fieldset role="group">
          <input
              v-model="newTask"
              type="text"
              placeholder="Nouvelle tâche"
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
              v-for="task in sortedTasks"
              :key="task.date"
              :class="{ completed: task.completed }"
          >
            <Checkbox
                :label="task.title"
                v-model="task.completed"
            />
          </li>
        </ul>

        <p v-if="remainingTasks > 0">
          {{ remainingTasks }} tâche{{ remainingTasks > 1 ? 's' : '' }} à faire
        </p>
      </div>
    </template>

    <template #footer>
      <small>&copy; 2025 - Application de gestion de tâches personnelle</small>
    </template>
  </Layout>
</template>

<script setup>
import {computed, onMounted, ref} from 'vue'
import Checkbox from "./Checkbox.vue";
import Button from "./Button.vue";
import Layout from "./Layout.vue";
import Timer from "./Timer.vue";

const newTask = ref('')
const hideCompleted = ref(false)
const tasks = ref([])
const showTimer = ref(true)

onMounted(() => {
  fetch('https://jsonplaceholder.typicode.com/todos')
      .then(r => r.json())
      .then(v => tasks.value = v.map(task => ({...task, date: task.id })))
})

const addTask = () => {
  tasks.value.push({
    title: newTask.value,
    completed: false,
    date: Date.now(),
  })
  newTask.value = ''
}

const sortedTasks = computed(() => {
  const sorted = tasks.value
      .slice()
      .sort((a, b) => (a.completed > b.completed ? 1 : -1))
  return hideCompleted.value ? sorted.filter(task => !task.completed) : sorted
})

const remainingTasks = computed(() => {
  return tasks.value.filter(t => !t.completed).length
})
</script>

<style>
.completed {
  opacity: 0.5;
  text-decoration: line-through;
}

aside {
  padding: 1rem;
  background-color: #f9f9f9;
}

aside h2 {
  margin-top: 0;
  font-size: 1.1rem;
}

aside ul {
  list-style: none;
  padding: 0;
  margin: 0 0 1rem 0;
}

aside li {
  margin: 0.3rem 0;
}
</style>
