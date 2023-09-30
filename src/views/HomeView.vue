<template>
  <div>
    <main>
      <!-- heading -->
      <header>
        <img src="../assets/pinia-logo.svg" alt="pinia logo">
        <h1>Pinia Tasks</h1>
      </header>

      <!-- new task form -->
      <div class="new-task-form">
        <TaskForm />
      </div>

      <!-- filter -->
      <nav class="filter">
        <button @click="filter = 'all'">All tasks</button>
        <button @click="filter = 'favs'">Fav tasks</button>
        <button @click="taskStore.$reset">Reset the state</button>
      </nav>

      <!-- loading -->
      <div class="loading" v-if="loading">Loading tasks...</div>

      <!-- task list -->
      <div class="task-list" v-if="filter === 'all'">
        <p>You have {{ totalCount }} tasks left to do.</p>
        <div v-for="task in tasks" :key="task.id">
          <TaskDetails :task="task" />
        </div>
      </div>

      <div class="task-list" v-if="filter === 'favs'">
        <p>You have {{ favCount }} tasks in your favs list.</p>
        <div v-for="task in favs" :key="task.id">
          <TaskDetails :task="task" />
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import TaskDetails from '../components/TaskDetails.vue';
import TaskForm from '../components/TaskForm.vue';
import { useTaskStore } from '../stores/TaskStore'
import { storeToRefs } from 'pinia'
import { ref, watchEffect } from 'vue'

const taskStore = useTaskStore()

const filter = ref('all')

const { tasks, loading, favs, favCount, totalCount } = storeToRefs(taskStore)

watchEffect(() => {
  // fetch tasks
  taskStore.getTasks()
})
</script>
