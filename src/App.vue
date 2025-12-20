<script setup lang="ts">
import { ref } from 'vue'
import { supabaseClient } from './supabase';


//import { RouterLink, RouterView } from 'vue-router'
//import HelloWorld from './components/HelloWorld.vue'

let inputText = ref('');


//insert input text message into siteMessages table
async function sendMessage(){
  await supabaseClient.from('siteMessages').insert({
    id: 0, 
    created_at: '',
    message: inputText.value
  })
}

async function send_message_cheating(){
  sendMessage();
}

</script>

<template>
  <input v-model="inputText">enter a message</input>
  <button>send message</button>
  <p>input value: {{inputText}}</p>   
</template>


