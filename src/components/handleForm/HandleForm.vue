<script setup lang="ts">
import { Form, Field, ErrorMessage } from 'vee-validate'
import { ref, defineEmits   } from 'vue';
import { defineRule } from 'vee-validate';
const input_content = ref('')
const input_category = ref(null)

const emit = defineEmits(['addnew'])

const addTodo = (value : any) => {
    if (value.content === '' || value.category === null) {
        return 'Content is required';
    }
    emit('addnew', value.content, value.category);
    input_content.value = '';
    input_category.value = null;
    return true;
}

defineRule('required', (value : any) => {
    if (value === '' || value === null) {
        return 'Content is required';
    }
    return true;
})

</script>
<template>
    <section class="create-todo">
        <h3>CREATE A TODO</h3>
        <Form @submit="addTodo">
            <h4>What's on your todo list</h4>   
            <Field name="content" type="text" placeholder="e.g make a video" rules="required" v-model="input_content"/>
            <ErrorMessage name="content" style="color: red"/>
            <h4>Pick a category </h4>
            <div class="options">
                <label>
                    <Field name="category" type="radio" value="business" v-model="input_category" rules="required"/>
                    <span class="bubble business"></span>
                    <div>Business</div> 
                </label>
                <label>
                    <Field name="category" type="radio" value="personal" v-model="input_category" rules="required" />
                    <span class="bubble personal"></span>   
                    <div>Personal</div>
                </label>
                <ErrorMessage name="category" style="color: red"/>
            </div>
            <input type="submit" value="Add todo">
        </Form>
    </section>
</template>