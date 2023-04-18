<script setup lang="ts">
import EditTodo from '../editTodo/EditTodo.vue'
import DeleteTodo from '../deleteTodo/DeleteTodo.vue'
import {defineProps, defineEmits ,ref} from 'vue'
import axios from 'axios';
const showEdit = ref(false);
const showRemove = ref(false);
const ID = ref<number>();
const props = defineProps({
    todos: Array<{id: number, content: string, category: string, done: boolean, createAt: number}>,
})

const emit = defineEmits(['update']);

const showRemoveTodo = (todo : {id: number}) => {
    showRemove.value = true;
    ID.value = todo.id;
}

const showEditTodo = (todo: {id: number}) => {
    showEdit.value = true;
    ID.value = todo.id;
}

const closeRemove = () => {
    showRemove.value = false;
}

const closeEdit = () => {
    showEdit.value = false;
}

const Edit = (content : any, category : any) => {
    console.log(content,category);
    axios.put(`https://641d13f31a68dc9e461685d0.mockapi.io/api/todos/note/${ID.value}`, {
        content,
        category
    })
    .then(res => {
        if (res.status === 200) {
            emit('update')
            alert('Edit success')
            showEdit.value = false;
        }
    })
    .catch(err => {
        console.log(err)
    })
}

const Remove = (id: number) => {
  axios.delete(`https://641d13f31a68dc9e461685d0.mockapi.io/api/todos/note/${id}`)
    .then(res => {
      if (res.status === 200) {
        alert('Delete success');
        emit('update');
        showRemove.value = false;
      }
      console.log(res.data);
    })
}

</script>
<template>
    <section class="todo-list">
        <h3>TODO LIST</h3>
        <div class="list">
            <div v-for="todo in props.todos" :key="todo.id" :class="`todo-item ${todo.done && 'done'}`">
                <label>
                    <input type="checkbox" v-model="todo.done" />
                    <span :class="`bubble ${todo.category}`"></span>
                </label>
                <div class="todo-content">
                    <input type="text" v-model="todo.content">
                </div>
                <div class="actions">
                    <button class="delete" @click="showRemoveTodo(todo)">Delete</button>
                    <button class="edit" @click="showEditTodo(todo)">Edit</button>
                </div>
            </div>  
        </div>
        <EditTodo v-if="showEdit" :id_Todo="ID" @close="closeEdit" @edit="Edit"/>
        <DeleteTodo v-if="showRemove" :id_Todo="ID" @close="closeRemove" @remove="Remove"/>   
    </section>
</template>


 