<script setup lang="ts">
import axios from 'axios';
import { defineProps, defineEmits, onBeforeMount, ref } from 'vue';
const props = defineProps({
    id_Todo: Number,
})
const todo = ref({
    content: '',
    category: ''
})

const emit = defineEmits(['close', 'remove'])

const closeModal = () => {
    emit('close')
}

const remove = () => {
    emit('remove', props.id_Todo)
}

const getData = () => {
    axios.get(`https://641d13f31a68dc9e461685d0.mockapi.io/api/todos/note/${props.id_Todo}`)
        .then(res => {
            todo.value = res.data
        })
        .catch(err => {
            console.log(err)
        })
}

onBeforeMount(() => {
    getData()
})

</script>

<template>
    <div class="modal-overlay" >
      <div class="modal">
        <h2>Bạn có chắc chắn muốn xóa?</h2>
        <div :class="`todo-item done`">
                <label>
                    <input type="checkbox" checked/>
                    <span :class="`bubble ${todo.category}`"></span>
                </label>
                <div class="todo-content">
                    <input type="text" v-model="todo.content">
                </div>
        </div>  
        <div class="modal-buttons">
          <button class="btn-yes" @click="remove">Yes</button>
          <button class="btn-cancel" @click="closeModal">Cancel</button>
        </div>
      </div>
    </div>
</template>


<style scoped lang="scss">
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10;
}

.modal {
  background: white;
  border-radius: 5px;
  padding: 20px;
  max-width: 400px;
  text-align: center;
}

.modal h2 {
  font-size: 24px;
  margin-bottom: 20px;
}

.modal-buttons {
  display: flex;
  justify-content: center;
}

.btn-yes,
.btn-cancel {
  background-color: transparent;
  border: 2px solid #000;
  color: #000;
  padding: 10px 20px;
  border-radius: 5px;
  margin: 0 10px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.btn-yes:hover,
.btn-cancel:hover {
  background-color: #000;
  color: #fff;
}


</style>


