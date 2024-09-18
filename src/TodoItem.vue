<template>
    <div :class="`todo-item ${todo.done && 'done'}`">
      <label>
        <input type="checkbox" v-model="todo.done" />
        <span :class="`bubble ${todo.tipo}`"></span>
      </label>
      <div class="todo-tarefa">
        <input type="text" v-model="todo.tarefa" />
      </div>
      <div class="actions">
        <button class="delete" @click="handleRemove">Delete</button>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps, defineEmits } from 'vue'



  const props = defineProps({
    todo: Object
  })
  
  const emit = defineEmits(['remove-todo'])
  
  const handleRemove = () => {
    emit('remove-todo', props.todo)
  }
  </script>
  