<template>
    <section class="greeting">
      <h2 class="title">
        What's up, <input type="text" placeholder="Name here" v-model="name" />
      </h2>
    </section>
</template>

<script setup>
import { ref, watch, onMounted } from 'vue'

const name = ref('')

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})


onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})


</script>