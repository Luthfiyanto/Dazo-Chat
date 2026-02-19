<script setup>
import { ref } from "vue";

defineProps({
  role: String,
  msg: String,
});
</script>
<template>
  <div :class="['chat', role == 'user' ? 'chat-end' : 'chat-start']">
    <div class="chat-header">{{ role }}</div>
    <div class="chat-bubble">{{ msg }}</div>
  </div>
</template>
