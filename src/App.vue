<script setup lang="ts">
import axios from 'axios'
import { ref, onBeforeMount } from 'vue'
import HandleForm from './components/handleForm/HandleForm.vue'
import ListTodo from './components/listTodo/ListTodo.vue'


const name = ref('')
// const todos = ref<{createAt: number}[]>([]);
const todos = ref([]) 
// const todos_asc = computed(() => todos.value.slice().sort((a,b) => a.createAt - b.createAt))

const getData = () => {
  axios.get('https://641d13f31a68dc9e461685d0.mockapi.io/api/todos/note')
    .then(res => {
      if (res.status === 200){
        todos.value = res.data
      }
      else{
        alert('Get data fail')  
      }
    })
    .catch(err => {
      console.error(err);
    })
}

onBeforeMount(() => {
  getData()  
})

const addTodo = (content :any, category: any) => {
  axios.post('https://641d13f31a68dc9e461685d0.mockapi.io/api/todos/note', {
    content,
    category,
    done: false,
    createAt: Date.now()
  })
    .then(res => {
      if (res.status === 201) {
        getData()
        alert('Add success')
      }
    })
    .catch(err => {
      alert('Add fail')
      console.log(err);
    })
}
</script> 

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What' up, <input type="text" v-model="name" placeholder="Your name" />
      </h2>
    </section>
    <HandleForm @addnew=addTodo ></HandleForm>
    <ListTodo :todos="todos" @update="getData"></ListTodo>
  </main>
</template>

