<template>
  <section class="todo-list">
    <h3>TODO LIST</h3>
    <div class="list">
      <TodoItem v-for="todo in todos" :key="todo.createdAt" :todo="todo" @remove-todo="removeTodo" />
    </div>
  </section>

  
</template>

<script setup>
import { defineProps, defineEmits } from 'vue'
import TodoItem from './TodoItem.vue'



const props = defineProps({
  todos: Array
})

const emit = defineEmits(['remove-todo'])

const removeTodo = (todo) => {
  emit('remove-todo', todo)
}
</script>
