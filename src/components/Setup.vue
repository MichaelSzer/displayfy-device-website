<script setup>
import { ref } from 'vue'

const emit = defineEmits(['setupDone'])

const ssid = ref('')
const password = ref('')

let connecting = false

const connect = () => {
    if(connecting) return
    connecting = true
  
    if (ssid.value === ''){
        alert("Please type the Wifi name.")
        return
    }else if (password.value === ''){
        alert("Please type the Wifi password.")
        return
    }

    // Send wifi params to Esp32 with a Post Http
    const formData = new FormData()
    formData.append('ssid', ssid.value)
    formData.append('password', password.value)
    const req = new XMLHttpRequest() // Make an HTTP Request without refreshing website https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest
    
    // Event when upload finish
    req.addEventListener('loadend', () =>{
      connecting = false
      emit('setupDone')
    })
    
    req.open('POST', '/set-wifi')
    req.send(formData)
}

</script>

<template>
    <div class="container">
        <h1 id="title">DisplayFy Setup</h1>
        <p style="font-size: medium;">Please fill out the form to connect DisplayFy to the internet.</p>

        <div style="margin-top: 2em; margin-bottom: 1em;">
        <div class="text-input">
            <p style="margin-right: 1em;">Wifi:</p><input placeholder="House-5G" type="text" v-model="ssid">
        </div>
        <div class="text-input" style="margin-top: 1em;">
            <p style="margin-right: 1em;">Password:</p><input placeholder="****" type="password" v-model="password">
        </div>
        </div>
        <button @click="connect">
        Connect
        </button>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

#title {
  font-size: 4em;
  margin-top: 0.5em;
}

.text-input {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
</style>