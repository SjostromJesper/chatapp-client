<script setup>
import {io} from 'socket.io-client'
import {ref} from "vue";

const username = prompt("Enter your username");

const socket = io('https://chatapp-server-production-85c9.up.railway.app')
const messages = ref([])

const inputField = ref(null)

console.log(socket)

socket.on('message', (username, message) => {
  messages.value.push({username, message})
})

const sendMessage = (event) => {
  const formData = new FormData(event.target)
  socket.emit('message', formData.get('message'))

  console.log(inputField.value.value)

  inputField.value.value = ''
}
</script>

<template>
  <div>
    <form @submit.prevent="sendMessage">
      <p v-for="message in messages">{{ username }}: {{ message }}</p>
      <input ref="inputField" type="text" name="message" placeholder="Enter message" required />
      <button>send</button>
    </form>

  </div>
</template>

<style scoped>
</style>
