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

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos') || [])
})

const addTodo = () => {
  if (inputContent.value.trim() === '' || inputCategory.value === null) return

  todos.value.push({
    content: inputContent.value,
    category: inputCategory.value,
    done: false,
    createdAt: new Date().getTime()
  })

  inputContent.value = ''
  inputCategory.value = null
}

const removeTodo = (todo) => todos.value = todos.value.filter((t) => t !== todo)

</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up <input type="text" placeholder="Name Here" v-model="name" />
      </h2>
    </section>
    <section class="create-todo">
      <h3>CREATE A TODO</h3>
      <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input type="text" placeholder="e.g. Learn React-Redux" v-model="inputContent">
        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input type="radio" name="category" value="business" id="category1" v-model="inputCategory">
            <span class="bubble business"></span>
            <div>Business</div>
          </label>
          <label>
            <input type="radio" name="category" value="personal" id="category2" v-model="inputCategory">
            <span class="bubble personal"></span>
            <div>Pesonal</div>
          </label>
        </div>
        <input type="submit" value="Add Todo">
      </form>
    </section>
    <section class="todo-list">
      <h3>TODO LIST</h3>
      <div class="list">
        <div v-for="todo in todosAsc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" name="" id="" v-model="todo.done">
            <span :class="`bubble ${todo.category} `"></span>
          </label>
          <div class="todo-content">
            <input type="text" name="" id="" v-model="todo.content">
          </div>
          <div class="actions">
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>