<template>
  <aside class="w-64 bg-gray-800 text-white flex flex-col p-4">
    <h2 class="text-xl font-semibold mb-4">Kape</h2>

    <button
      @click="clearChat"
      class="w-full bg-gray-700 hover:bg-red-600 text-white py-2 px-4 rounded mb-4"
    >
      Clear Chat
    </button>

    <div class="flex-1 overflow-y-auto space-y-2">
      <div
        v-for="(chat, index) in conversations"
        :key="index"
        class="p-3 bg-gray-700 rounded-lg cursor-pointer hover:bg-gray-600"
      >
        {{ chat }}
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  data() {
    return {
      conversations: [
        "New Chat",
        "Previous Conversation 1",
        "Previous Conversation 2",
      ],
    }
  },
  methods: {
    clearChat() {
      this.$emit("clear-chat")
    },
  },
}
</script>
