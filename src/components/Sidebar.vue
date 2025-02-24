<template>
  <aside
    :class="[
      'bg-gray-800 text-white flex flex-col p-4 h-screen fixed md:relative z-20 transition-transform',
      sidebarOpen ? 'w-64 translate-x-0' : 'w-0 -translate-x-full',
    ]"
  >
    <button @click="$emit('toggle-sidebar')" class="self-end text-xl">×</button>

    <h2 class="text-xl font-semibold mb-4">Kape</h2>

    <button
      @click="$emit('clear-chat')"
      class="w-full bg-gray-700 hover:bg-red-600 text-white py-2 px-4 rounded mb-4 transition"
    >
      Clear Chat
    </button>

    <button
      @click="$emit('new-chat')"
      class="w-full bg-blue-500 hover:bg-blue-600 text-white py-2 px-4 rounded mb-4 transition"
    >
      + New Chat
    </button>

    <div class="flex-1 overflow-y-auto space-y-2">
      <h3 class="text-lg font-semibold mb-2">Previous Conversations</h3>
      <div
        v-for="(chat, index) in conversations"
        :key="index"
        class="p-3 bg-gray-700 rounded-lg cursor-pointer hover:bg-gray-600 transition"
        @click="$emit('load-chat', index)"
      >
        {{ chat.name }}
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  props: ["sidebarOpen"],
  data() {
    return {
      conversations: JSON.parse(localStorage.getItem("conversations")) || [
        { name: "New Chat", messages: [] },
      ],
    }
  },
}
</script>
