<template>
    <form @submit.prevent="handleSubmit">
        <input
            type="text"
            placeholder="I need to..."
            v-model="newTask"
        >
        <button>Add</button>
    </form>
</template>

<script setup>
import { ref } from 'vue'
import { useTaskStore } from '../stores/TaskStore';

const taskStore = useTaskStore()

const newTask = ref('')

const handleSubmit = () => {
    if (newTask.value) {
        taskStore.addTask({
            title: newTask.value,
            isFav: false,
            id: taskStore.tasks.length + 1
        })
        newTask.value = ''
    }
}
</script>
