<script setup lang="ts">
import { computed, nextTick, onMounted, onUpdated, ref } from 'vue'
import ChatMessage from './ChatMessage.vue'
import { useChats } from '../services/chat.ts'
import { debugMode } from '../services/appConfig.ts'

const { messages } = useChats()
const chatElement = ref<HTMLElement>()

const scrollToBottom = () => {
  nextTick(() => {
    if (chatElement.value) {
      chatElement.value.scrollTop = chatElement.value.scrollHeight
    }
  })
}

onMounted(() => scrollToBottom())
onUpdated(() => scrollToBottom())

const visibleMessages = computed(() =>
  debugMode.value ? messages?.value : messages?.value.filter((m) => m.role != 'system'),
)
</script>

<template>
  <div
    ref="chatElement"
    class="flex-1 overflow-y-auto rounded-xl bg-zinc-100 p-4 text-sm leading-6 text-zinc-900 dark:bg-zinc-800 dark:text-zinc-300 sm:text-base sm:leading-7 scroll-smooth"
  >
    <ChatMessage v-for="message in visibleMessages" :message="message" />
  </div>
</template>
