<script setup>
import {io} from 'socket.io-client'
import {ref} from "vue";

const socket = io('https://chatapp-server-production-85c9.up.railway.app')
const messages = ref([])

console.log(socket)

socket.on('message', message => {
  messages.value.push(message)
})

const sendMessage = (event) => {
  const formData = new FormData(event.target)
  socket.emit('message', formData.get('message'))
}
</script>

<template>
  <div>
    <form @submit.prevent="sendMessage">
      <p v-for="message in messages">{{ message }}</p>
      <input type="text" name="message" placeholder="Enter message" required />
      <button>send</button>
    </form>

  </div>
</template>

<style scoped>
</style>
