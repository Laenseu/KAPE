<template>
  <div class="flex h-screen bg-gray-100">
    <Sidebar @clearChat="clearChat" />
    <ChatWindow :messages="messages" @sendMessage="sendMessage" />
  </div>
</template>

<script>
import { ref, onMounted, watch } from "vue"
import Sidebar from "./components/Sidebar.vue"
import ChatWindow from "./components/ChatWindow.vue"

export default {
  components: { Sidebar, ChatWindow },
  setup() {
    const messages = ref([])

    // Load messages from localStorage
    onMounted(() => {
      const storedMessages = localStorage.getItem("chatHistory")
      if (storedMessages) {
        messages.value = JSON.parse(storedMessages)
      }
    })

    // Watch for changes and save to localStorage
    watch(
      messages,
      (newMessages) => {
        localStorage.setItem("chatHistory", JSON.stringify(newMessages))
      },
      { deep: true }
    )

    const sendMessage = (message) => {
      messages.value.push({ text: message, sender: "user" })

      // Simulated AI response
      setTimeout(() => {
        messages.value.push({
          text: "This is a simulated AI response.",
          sender: "ai",
        })
      }, 1000)
    }

    const clearChat = () => {
      messages.value = []
      localStorage.removeItem("chatHistory")
    }

    return { messages, sendMessage, clearChat }
  },
}
</script>
