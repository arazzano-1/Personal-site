<script setup lang="ts">
import { ref } from 'vue'

//import { RouterLink, RouterView } from 'vue-router'
//import HelloWorld from './components/HelloWorld.vue'

let inputText = ref('');

</script>

<template>
  <input v-model="inputText">enter a message</input>
  <button>send message</button>
  <p>input value: {{inputText}}</p>   
</template>


