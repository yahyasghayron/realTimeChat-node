<template>
  <div id="app">
    <h1>Real-Time Chat</h1>
    <div v-if="!inChat">
      <input v-model="room" placeholder="Enter room name" />
      <button @click="enterChat">Join Room</button>
    </div>
    <ChatRoom v-if="inChat" :room="room" />
  </div>
</template>

<script>
import ChatRoom from "../components/ChatRoom.vue";

export default {
  data() {
    return {
      room: "",
      inChat: false,
    };
  },
  components: {
    ChatRoom,
  },
  methods: {
    enterChat() {
      if (this.room.trim() === "") return;
      this.inChat = true;
    },
  },
};
</script>

