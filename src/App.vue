<script setup>
import { ref, watch } from "vue";

const todos = ref(JSON.parse(localStorage.getItem("todos") || "[]"));
const newTodo = ref("");

const addToDo = () => {
  if (newTodo.value.trim() !== "") {
    todos.value.push({
      title: newTodo.value,
      completed: false,
      id: Date.now(),
    });
  }
  newTodo.value = "";
};

const delTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id);
};

watch(
  todos,
  (newVal) => {
    localStorage.setItem("todos", JSON.stringify(newVal));
  },
  { deep: true }
);
</script>
<template>
  <div class="relative w-90 h-96 bg-indigo-700 shadow-2xl rounded-2xl p-3">
    <div class="fixed flex flex-col w-80">
      <h1 class="text-gray-400 text-center font-bold italic uppercase p-1">
        To Do List VUEJS
      </h1>
      <div class="w-full flex gap-1 justify-center">
        <input
          type="text"
          v-model="newTodo"
          placeholder="Ajouter Une Nouvelle Tâche"
          class="bg-gray-200 outline-none rounded p-1.5 w-3/4"
        />
        <input
          @click="addToDo"
          type="submit"
          value="Ajouter"
          class="border border-indigo-50 rounded bg-indigo-200 p-1 w-3/12 font-semibold"
        />
      </div>
    </div>
    <div class="absolute w-90 h-72 top-20 left-0 overflow-y-auto">
      <div v-if="todos.length === 0">
        <h3 class="text-center text-white mt-4">
          Aucune Tâches Ajouter pour l'instant !
        </h3>
      </div>
      <div v-else>
        <ul>
          <li
            v-for="todo in todos"
            :key="todo.id"
            class="flex gap-2 items-center justify-between p-1.5 m-2 transiton duration-150 ease-in-out hover:bg-indigo-500 rounded"
          >
            <div class="flex gap-1.5">
              <input
                type="checkbox"
                name=""
                v-model="todo.completed"
                :id="`t-${todo.id}`"
                class="cursor-pointer"
              />
              <label :for="`t-${todo.id}`" class="font-semibold">
                {{ todo.title }}
              </label>
            </div>
            <button
              @click="delTodo(todo.id)"
              class="flex items-center justify-center hover:bg-indigo-300 p-1 rounded"
            >
              <ion-icon name="trash-outline"></ion-icon>
            </button>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
