<template>
  <div class="form-container">
    <h2>Contact Us</h2>
    <form @submit.prevent="submitForm">
      <label>Name:</label>
      <input type="text" v-model="form.name" required />

      <label>Email:</label>
      <input type="email" v-model="form.email" required />

      <label>Message:</label>
      <textarea v-model="form.message" rows="4" required></textarea>

      <button type="submit">Send Message</button>
    </form>

    <p v-if="responseMessage" class="success">{{ responseMessage }}</p>
    <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import axios from 'axios'

const form = reactive({
  name: '',
  email: '',
  message: ''
})

const responseMessage = ref('')
const errorMessage = ref('')

const submitForm = async () => {
  try {
    const res = await axios.post('https://contact-form-test-iw4r.onrender.com/api/contact', form)
    responseMessage.value = res.data.message
    errorMessage.value = ''
    form.name = ''
    form.email = ''
    form.message = ''
  } catch (err) {
    responseMessage.value = ''
    errorMessage.value = err.response?.data?.message || 'Something went wrong!'
  }
}
</script>

<style scoped>
.form-container {
  max-width: 500px;
  margin: 50px auto;
  padding: 30px;
  background: #1f1f1f;
  color: #fff;
  border-radius: 10px;
  box-shadow: 0 0 10px #000;
  font-family: sans-serif;
}
form {
  display: flex;
  flex-direction: column;
}
label {
  margin-top: 15px;
  margin-bottom: 5px;
}
input,
textarea {
  padding: 10px;
  border-radius: 5px;
  border: none;
  background: #333;
  color: white;
}
button {
  margin-top: 20px;
  padding: 12px;
  background-color: #4caf50;
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
.success {
  margin-top: 20px;
  color: #4caf50;
}
.error {
  margin-top: 20px;
  color: red;
}
</style>
