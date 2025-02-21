<template>
  <div class="flex h-screen bg-gray-900 text-white">
    <!-- Sidebar -->
    <Sidebar />

    <!-- Chat Area -->
    <div class="flex-1 flex flex-col">
      <ChatWindow />
    </div>
  </div>
</template>

<script>
import ChatWindow from "../components/ChatWindow.vue"
import Sidebar from "../components/Sidebar.vue"

export default {
  components: {
    ChatWindow,
    Sidebar,
  },
}
</script>
