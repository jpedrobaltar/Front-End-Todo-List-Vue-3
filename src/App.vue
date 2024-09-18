<template>
  <main class="app">
    <Greeting />
    <CreateTodo @add-todo="addTodo"/>
    <TodoList />
    <section class="todo-list">
      <div class="list">
        <div 
          v-for="todo in todos_asc" 
          :key="todo.createdAt" 
          :class="`todo-item ${todo.done && 'done'}`"

        >
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${todo.tipo}`"></span>

          </label>

          <div class="todo-tarefa">
            <input type="text" v-model="todo.tarefa" /> 
            <div v-if="todo.data">
              <p>Date: {{ new Date(todo.data).toLocaleDateString() }}</p> 
            </div>
          </div>

          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script setup>
import { ref, onMounted, computed, watch } from 'vue'
import Greeting from './Greeting.vue'
import CreateTodo from './CreateTodo.vue'
import TodoList from './TodoList.vue'
import api from './services/api'

const todos = ref([])
const name = ref('')

const todos_asc = computed(() => {
  return todos.value.sort((a, b) => {
    return b.createdAt - a.createdAt
  })
})

const addTodo = (todo) => {
  
  api.post('/todo', todo)
  .then((response) => {
    todo._id = response.data.id
    todos.value.push(todo)
    console.log('Todo adicionado no servidor')

  }).catch((error) => {
    console.log('error')
  })
  }


const removeTodo = (todo) => {
  todos.value = todos.value.filter(t => t !== todo)
  api.delete(`/todo/${todo._id}`)
    .then(() => {
      console.log("Todo deletado no servidor")
    })
    .catch((error) => {
      console.log("Erro ao deletar o todo no servidor", error)
    })
}

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})



onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
  name.value = localStorage.getItem('name') || ''
  api.get('/todo')
  .then((response) => {
    console.log(response)
    todos.value = response.data
  }).catch((error) => {
    console.log("deu erro")
})
})
</script>


