<template>
  <div class="flex h-screen bg-gray-100">
    <!-- Sidebar -->
    <Sidebar
      :sidebarOpen="sidebarOpen"
      @toggle-sidebar="toggleSidebar"
      @update-chat="updateMessages"
      @clear-chat="clearChat"
      @new-chat="newChat"
      @load-chat="loadChat"
    />

    <!-- Chat Window with Dynamic Width -->
    <ChatWindow
      :messages="messages"
      :isSidebarOpen="sidebarOpen"
      @sendMessage="sendMessage"
      @toggle-sidebar="toggleSidebar"
    />
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
    const sidebarOpen = ref(true)
    const conversations = ref(
      JSON.parse(localStorage.getItem("conversations")) || [
        { name: "New Chat", messages: [] },
      ]
    )
    const activeChat = ref(0)

    onMounted(() => {
      messages.value = conversations.value[activeChat.value]?.messages || []
    })

    watch(
      messages,
      (newMessages) => {
        conversations.value[activeChat.value].messages = newMessages
        localStorage.setItem(
          "conversations",
          JSON.stringify(conversations.value)
        )
      },
      { deep: true }
    )

    const updateMessages = (newMessages) => {
      messages.value = [...newMessages]
    }

    const sendMessage = (message) => {
      if (!message.trim()) return
      messages.value.push({ text: message, sender: "user" })

      setTimeout(() => {
        messages.value.push({
          text: "This is a simulated AI response.",
          sender: "ai",
        })
      }, 1000)
    }

    const toggleSidebar = () => {
      sidebarOpen.value = !sidebarOpen.value
    }

    const clearChat = () => {
      conversations.value = [{ name: "New Chat", messages: [] }]
      activeChat.value = 0
      messages.value = []
      localStorage.setItem("conversations", JSON.stringify(conversations.value))
    }

    const newChat = () => {
      const newChat = {
        name: `New Chat ${conversations.value.length + 1}`,
        messages: [],
      }
      conversations.value.unshift(newChat)
      activeChat.value = 0
      messages.value = newChat.messages
      localStorage.setItem("conversations", JSON.stringify(conversations.value))
    }

    const loadChat = (index) => {
      activeChat.value = index
      messages.value = conversations.value[index].messages
    }

    return {
      messages,
      sendMessage,
      updateMessages,
      toggleSidebar,
      sidebarOpen,
      clearChat,
      newChat,
      loadChat,
    }
  },
}
</script>
