<script setup>
import { ref } from 'vue';

const tasks = ref([
  { name: 'Task 1', time: 60 },
  { name: 'Task 2', time: 75 },
  { name: 'Task 3', time: 80 },
]);

const showModal = ref(false);
const taskName = ref('');
const taskTime = ref();
const showAlert = ref(false);

function modal() {
  showModal.value = true;
}

function hideModal() {
  showModal.value = false;
}

function removeTask(id) {
  tasks.value.splice(id, 1);
}

function addNewTask() {
  if (taskName.value !== '' && taskTime.value > 0) {
    tasks.value.push({ name: taskName.value, time: taskTime.value });
    taskName.value = '';
    taskTime.value = 0;
    showModal.value = false;
    if (showAlert.value) {
      showAlert.value = false;
    }
  } else {
    showAlert.value = true;
  }
}
</script>


<template>
    <!-- Tasks List -->
    <div class="flex items-center justify-center min-h-screen">
        <div class="max-w-3xl mx-5 w-full p-6 rounded-xl shadow-xl bg-teal-500">
            <div class="flex justify-end items-center mb-2">
                <button class="btn btn-active btn-neutral" @click="modal()">Add New Task</button>
            </div>
            <h1 class="text-3xl font-bold text-center text-white">Tasks List</h1>
            <div v-if="tasks.length > 0" v-for="(task, index) in tasks" :key="index" class="flex justify-between items-center border-b-2 py-2 bg-white p-5 rounded-lg my-3">
                <span class="text-md font-bold">{{ task.name }} - {{ task.time }} minutes</span>
                <button @click="removeTask(index)" class="btn btn-error"><i class="fa fa-trash text-white"></i></button>
            </div>
            <div v-else class="flex justify-center items-center border-b-2 py-2 h-36 bg-green-300 p-5 rounded-lg my-3">
                <h1 class="font-bold text-2xl">You have no task.</h1>
            </div>
        </div>
    </div>

    <!-- Add Task Modal -->
    <div v-if="showModal" class="fixed top-0 left-0 w-full h-full flex items-center justify-center">
      <div @click="hideModal" class="absolute top-0 left-0 w-full h-full bg-gray-900 opacity-50"></div>
      <div class="z-10 lg:w-1/3 md:w-1/2 sm:w-1/2 w-full mx-5 max-w-3xl p-4 bg-white rounded-md">
        <div v-if="showAlert" class="my-1 p-2 bg-red-500 text-white rounded-md">
            Please fill in all required fields.
        </div>
        <button @click="hideModal" class="absolute top-0 right-0 p-4" ><i class="fa fa-times text-gray-700"></i></button>
        <h2 class="text-2xl font-bold mb-4">Add New Task</h2>
          <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">Task Name</label>
            <input type="text" required v-model="taskName" class="mt-1 p-2 w-full border rounded-md">
          </div>
          <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700">Task Time (minutes)</label>
            <input type="number" required v-model="taskTime" @keypress.enter="addNewTask()" class="mt-1 p-2 w-full border rounded-md">
          </div>
          <button @click="addNewTask()" class="btn btn-active btn-neutral">Add Task</button>
      </div>
    </div>
    
</template>

<style scoped>

</style>
