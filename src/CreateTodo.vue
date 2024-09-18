<template>
  <section class="create-todo">
    <h3>Create a Todo</h3>
    <form @submit.prevent="handleSubmit">
      <h4>What's on your todo list?</h4>
      <input 
        type="text" 
        placeholder="Escreva uma tarefa"
        v-model="input_tarefa" 
      />
      <div class="options">
        <label>
          <input 
            type="radio"
            name="tipo" 
            value="business"
            v-model="input_tipo" 
          />
          <span class="bubble business"></span>
          <div>Business</div>
        </label>
        
        <label>
          <input 
            type="radio"
            name="tipo" 
            value="personal"
            v-model="input_tipo" 
          />
          <span class="bubble personal"></span>
          <div>Personal</div>
        </label>
        <VueDatePicker v-model="data" placeholder="Selecione uma data" text-input />
      </div>
      <input type="submit" value="Add Todo" />
    </form>
  </section>
</template>

<script setup>
import { ref, defineEmits } from 'vue'
import VueDatePicker from '@vuepic/vue-datepicker'
import '@vuepic/vue-datepicker/dist/main.css'


const data = ref();
const input_tarefa = ref('')
const input_tipo = ref(null)
const emit = defineEmits(['add-todo'])

const handleSubmit = () => {
  if (input_tarefa.value.trim() === '' || input_tipo.value === null) {
    return
  }
 
  emit('add-todo', {
    tarefa: input_tarefa.value,
    tipo: input_tipo.value,
    data: data.value, 
    done: false,
    createdAt: new Date().getTime(),
  })

  input_tarefa.value = ''
  input_tipo.value = null
  data.value = null 
}
</script>

