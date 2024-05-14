<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
    {id: id++, text: 'test', done: true, editing: false},
    {id: id++, text: 'learn vue', done: true, editing: false},
    {id: id++, text: 'test lagi', done: false, editing: false}
])

function tambah(){
  todos.value.push({id: id++, text: newTodo.value, done: false, editing: false})
  newTodo.value=''
}

function hapus(todo){
  todos.value = todos.value.filter((t) => t !== todo)
}

function edit(todo){
  todo.editing = true
}

function save(todo){
  todo.editing = false
}

const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})
</script>

<template>
  <form @submit.prevent="tambah">
    <input v-model="newTodo" placeholder="input here">
    <button type="submit">Tambah</button>
  </form>

  <ol>
    <li v-for="todo in filteredTodos" :key="todo.id">
      <input type="checkbox" v-model="todo.done">
      <span v-if="!todo.editing" :class="{done: todo.done}">{{ todo.text }}</span>
      <input v-else v-model="todo.text" @keyup.enter="save(todo)" @blur="save(todo)">
      <button v-if="!todo.editing" @click="edit(todo)">edit</button>
      <button @click="hapus(todo)">hapus</button>
    </li>
  </ol>

  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Show all' : 'Hide completed' }}
  </button>
</template>

<style>
.done {
  text-decoration: line-through;
}
</style>
