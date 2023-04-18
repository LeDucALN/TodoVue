<script lang="ts" setup>
import axios from 'axios';
import { Field, Form } from 'vee-validate';
import { defineProps, ref, defineEmits, onBeforeMount} from 'vue';

const props = defineProps({
  id_Todo: Number
})

const emit = defineEmits(['close', 'edit'])
const editTodo = (value : any) => {
  emit('edit', value.content, value.category)
}
const closeModal = () => {
    emit('close')
}

const todo = ref({
  content: '',
  category: ''
})

const getTodoById = () => {
  axios 
    .get(`https://641d13f31a68dc9e461685d0.mockapi.io/api/todos/note/${props.id_Todo}`)
    .then(res => {
      todo.value = res.data     
    })
    .catch(err => {
      console.log(err)
    })
}


onBeforeMount(async() => {
  getTodoById()
})

</script>

<template>
    <div class="modal">
        <div class="modal-content">
          <span class="close" @click=closeModal>&times;</span>
          <Form @submit="editTodo">
            <Field class="content" name="content" type="text" v-model="todo.content"/> 
            <br/>
            <div class="options">
                <label>
                    <Field name="category" type="radio" value="business" v-model="todo.category"/> 
                    <span class="bubble business"></span>
                    <div>Business</div>
                </label>
                <label>
                    <Field name="category" type="radio" value="personal" v-model="todo.category"/>
                    <span class="bubble personal"></span>   
                    <div>Personal</div>
                </label>
            </div>
            <button class="save">Lưu</button>
        </Form>
        </div>
    </div>
</template>

<style scoped lang="scss">
.modal {
    display: block; /* Mặc định ẩn modal */
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.4); /* Màu nền */
  }

  .modal-content {
    background-color: #f0f0f0;
    margin: 10% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 50%;
  }

  .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
  }
  
  .close:hover,
  .close:focus {
    color: #000;
    text-decoration: none;
    cursor: pointer;
  }

  input[type="text"] {
    width: 50%;
    border: 1px solid #ccc;
    border-radius: 5px;
    padding: 5px;
    background-color: #888;
  }

    .save {
    background-color: #4CAF50;
    color: white;
    padding: 5px 10px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .options {
    margin-top: 10px;
  }

</style>