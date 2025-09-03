<script setup lang="ts">
import { ref } from "vue";

const todos = ref([
    {
      id: 1,
      title: "learn vue.js",
    },
    {
      id: 2,
      title: "learn react.js",
    },
  ]),
  title = ref("");
let nextId = 3;

function addTodo() {
  todos.value.push({
    id: nextId++,
    title: title.value,
  });
  title.value = "";
}
</script>
<template>
  <div>
    <h4 class="my-1 text-center">代办列表</h4>
    <div>
      <input
        class="w-full px-4 py-2 border-black border-2 rounded-full "
        type="text"
        v-model="title"
        placeholder="请输入代办标题并按回车"
        @keydown.enter="addTodo"
      />
    </div>
    <ul class="h-60 overflow-y-scroll">
      <li
        class="flex items-center justify-between py-2"
        v-for="(todo, index) in todos"
        :key="todo.id"
      >
        <div class="first-letter:uppercase">
          {{ todo.title }}
        </div>
        <button class="btn btn-xs btn-secondary" @click="todos.splice(index, 1)">
          完成
        </button>
      </li>
    </ul>
  </div>
</template>
<style>
.first-upper-case {
  text-transform: capitalize;
}
</style>
