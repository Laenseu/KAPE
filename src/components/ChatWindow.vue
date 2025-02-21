<template>
  <div class="flex flex-col h-full bg-gray-900">
    <!-- Chat Messages -->
    <div class="flex-1 overflow-y-auto p-4 space-y-4">
      <div
        v-for="(message, index) in messages"
        :key="index"
        class="flex items-start space-x-3"
        :class="{ 'flex-row-reverse': message.sender === 'user' }"
      >
        <div
          class="w-7xl h-10 rounded-full bg-gray-700 flex items-center justify-center"
        >
          <span v-if="message.sender === 'user'">👤</span>
          <span v-else>🤖</span>
        </div>

        <p
          class="px-4 py-2 rounded-lg max-w-xs"
          :class="{
            'bg-blue-500 text-white': message.sender === 'user',
            'bg-gray-700 text-white': message.sender === 'ai',
          }"
        >
          {{ message.text }}
        </p>
      </div>
    </div>

    <!-- Input Box -->
    <div class="p-4 bg-gray-800 flex items-center">
      <input
        v-model="inputMessage"
        @keyup.enter="sendMessage"
        class="flex-1 p-2 bg-gray-700 text-white border-none rounded-lg outline-none"
        placeholder="Type a message..."
      />
      <button
        @click="sendMessage"
        class="ml-2 bg-blue-500 px-4 py-2 rounded-lg"
      >
        ➤
      </button>
    </div>
  </div>
</template>

<script>
import { ref } from "vue"

export default {
  setup() {
    const messages = ref([
      { text: "Hello! How can I help you today?", sender: "ai" },
    ])
    const inputMessage = ref("")

    const sendMessage = () => {
      if (!inputMessage.value.trim()) return

      messages.value.push({ text: inputMessage.value, sender: "user" })
      inputMessage.value = ""

      setTimeout(() => {
        messages.value.push({
          text: "This is a simulated AI response.",
          sender: "ai",
        })
      }, 1000)
    }

    return { messages, inputMessage, sendMessage }
  },
}
</script>
