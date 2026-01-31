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

const messages = ref(['']);

async function fetchMessages() {
  const { data, error } = await supabaseClient
    .from('siteMessages')
    .select('message');

  if (error) {
    console.error('Error fetching messages:', error);
    messages.value = [];
  }
  else {
    console.log(data.length);
    messages.value = data.map((item: { message: string }) => item.message);
    console.log([...messages.value]);
  }

}
function getMessageBoardStyle() {
  return {
    backgroundColor: 'blue',
    padding: '16px',
    borderRadius: '8px',
    color: 'white'
  };
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
  <button @click="fetchMessages">fetch Messages</button>  
</template>




