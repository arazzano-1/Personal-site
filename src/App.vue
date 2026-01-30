<script setup lang="ts">
import { ref } from 'vue'
import { supabaseClient } from './supabase';


//import { RouterLink, RouterView } from 'vue-router'
//import HelloWorld from './components/HelloWorld.vue'

let inputText = ref('');


//insert input text message into siteMessages table
async function sendMessage(){
  await supabaseClient.from('siteMessages').insert({
    message: inputText.value
  })
}

// Get the highest id from siteMessages table
async function getHighestId() {
  const { data, error } = await supabaseClient
    .from('siteMessages')
    .select('id')
    .order('id', { ascending: false })
    .limit(1)
    .single();

  if (error) {
    console.error('Error fetching highest id:', error);
    return null;
  }
  return data ? data.id : null;
}

//async function send_message_cheating(){
//   sendMessage();
// }

</script>

<template>
  <input v-model="inputText">enter a message</input>
  <button @click="sendMessage">send message</button>
  <p>input value: {{inputText}}</p>   
</template>


