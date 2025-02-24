<template>
  <div
    class="flex flex-col h-screen bg-gray-900 text-white transition-all duration-300"
    :class="{ 'w-full': !isSidebarOpen, 'w-[calc(100%-16rem)]': isSidebarOpen }"
  >
    <button
      @click="$emit('toggle-sidebar')"
      class="p-2 bg-gray-700 text-white fixed top-4 left-4 z-10 rounded"
    >
      ☰
    </button>

    <div class="flex-1 overflow-y-auto p-4 space-y-4" ref="chatContainer">
      <div
        v-for="(message, index) in messages"
        :key="index"
        class="flex items-start space-x-3"
        :class="{ 'flex-row-reverse': message.sender === 'user' }"
      >
        <div
          class="flex items-center space-x-2"
          :class="{ 'flex-row-reverse': message.sender === 'user' }"
        >
          <div
            class="w-10 h-10 rounded-full bg-gray-700 flex items-center justify-center"
          >
            <span v-if="message.sender === 'user'">👤</span>
            <span v-else>🤖</span>
          </div>
          <p
            class="px-4 py-2 rounded-lg max-w-[80%] sm:max-w-xs break-words"
            :class="{
              'bg-blue-500 text-white self-end': message.sender === 'user',
              'bg-gray-700 text-white self-start': message.sender === 'ai',
            }"
          >
            {{ message.text }}
          </p>
        </div>
      </div>
    </div>

    <div
      class="p-4 bg-gray-800 flex items-center border-t border-gray-700 w-full"
    >
      <input
        v-model="inputMessage"
        @keyup.enter="sendMessage"
        class="flex-1 p-2 bg-gray-700 text-white border-none rounded-lg outline-none"
        placeholder="Type a message..."
      />
      <button
        @click="sendMessage"
        class="ml-2 bg-blue-500 px-4 py-2 rounded-lg hover:bg-blue-600 transition"
      >
        ➤
      </button>
    </div>
  </div>
</template>

<script>
import { ref, nextTick } from "vue"

export default {
  props: ["messages", "isSidebarOpen"],
  setup(_, { emit }) {
    const inputMessage = ref("")
    const chatContainer = ref(null)

    const sendMessage = () => {
      if (!inputMessage.value.trim()) return

      emit("sendMessage", inputMessage.value)
      inputMessage.value = ""

      nextTick(() => {
        chatContainer.value.scrollTop = chatContainer.value.scrollHeight
      })
    }

    return { inputMessage, sendMessage, chatContainer }
  },
}
</script>
