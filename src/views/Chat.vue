<script setup>
import { ref } from "vue";
import ChatBubble from "../components/ChatBubble.vue";

const apiUrl = import.meta.env.VITE_API_URL;

const input = ref("");
const message = ref([]);
const loading = ref(false);

const sendMessage = async () => {
  if (!input.value) return;

  const userMsg = input.value;
  message.value.push({ role: "user", text: userMsg });
  input.value = "";
  loading.value = true;

  const res = await fetch(`${apiUrl}/chat`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ message: userMsg }),
  });

  const data = await res.json();

  message.value.push({ role: "ai", text: data.reply });
  loading.value = false;
};
</script>

<template>
  <section class="grid grid-cols-1 md:grid-cols-2">
    <div class="chat p-7 bg-slate-800 min-h-screen">
      <div v-for="(m, i) in message" :key="i">
        <ChatBubble :role="m.role" :msg="m.text" />
      </div>

      <div v-if="loading">AI is typing...</div>

      <div class="flex justify-between gap-2 absolute bottom-0 w-2/3 md:w-1/3 my-7">
        <input v-model="input" @keyup.enter="sendMessage" class="input input-primary w-full" />
        <button @click="sendMessage" class="btn btn-primary btn-soft">Send</button>
      </div>
    </div>
    <div></div>
  </section>
</template>
