<script setup>
import { ref, onMounted, watch, computed } from 'vue'

const todos = ref([])
const name = ref('')
const inputContent = ref('')
const inputCategory = ref(null)

const todosAsc = computed(() => todos.value.sort((a, b) => {
  return b.crreatedAt - a.crreatedAt
}))

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
})

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up <input type="text" placeholder="Name Here" v-model="name" />
      </h2>
    </section>
  </main>
</template>