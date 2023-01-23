<template>
  <main>
    <div id="chat_container">
      <div class="header">ChatGPT</div>
      <div
        v-for="(chat, i) in wrapper"
        :key="i"
        class="wrapper"
        :class="{ ai: chat.isAi }"
      >
        <Chat :chat="chat" :key="i" />
      </div>
    </div>
    <form @submit.prevent="fetchAnswer">
      <input type="text" placeholder="Ask ChatGPT...Use image text for image!" v-model="question" />
      <button type="submit"><img src="@/assets/send.svg" alt="send" /></button>
    </form>
  </main>
</template>
<script setup>
import { ref } from "vue";
import Chat from "@/components/Chat.vue";
const question = ref("");
const wrapper = ref([]);
const loading = ref(false);

const fetchAnswer = async () => {
  try {
    loading.value = true;
    wrapper.value.push({
      isAi: false,
      value: question.value,
      isImg: false
    });
    wrapper.value.push({
      isAi: true,
      value: "Loading....",
      isImg: false
    });
    const apiURL =  question.value.includes("image") ? "image" : "text";
    const res = await fetch(`http://localhost:8000/api/${apiURL}`, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        question: question.value,
      }),
    });
    const data = await res.json();
    //console.log(data);
    const parsedData = data.bot.trim();
    wrapper.value.pop();
    wrapper.value.push({
      isAi: true,
      value: parsedData,
      isImg: apiURL === "image"
    });
  } catch (error) {
  } finally {
    loading.value = false;
    question.value = "";
  }
};
</script>
