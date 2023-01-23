<template>
  <div class="chat" ref="currentMessage">
    <div class="profile">
      <img v-if="chat.isAi" src="@/assets/bot.svg" alt="" />
      <img v-else src="@/assets/user.svg" alt="" />
    </div>
    <div class="message">
      <span v-if="!chat.isImg">{{ chat.value }}</span>
      <img :src="chat.value" v-if="chat.isImg" />
    </div>
  </div>
</template>
  
  <script setup>
import { nextTick, onMounted, ref, watch } from "vue";
defineProps({
  chat: {
    type: Object || null,
  },
  api: {
    type: String || null,
  },
  ai: {
    type: Boolean,
  },
});
const currentMessage = ref(null);
onMounted(async () => {
  await nextTick();
  const div = currentMessage.value;
  div.scrollIntoView({ behavior: "smooth" });
});
</script>