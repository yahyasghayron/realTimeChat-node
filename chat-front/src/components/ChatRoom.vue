<template>
  <div>
    <h2>Room: {{ room }}</h2>
    <div class="messages">
      <div v-for="(message, index) in messages" :key="index">
        <strong>{{ message.sender }}:</strong> {{ message.message }}
      </div>
    </div>
    <input
      type="text"
      v-model="message"
      placeholder="Type a message"
      @keyup.enter="sendMessage"
    />
  </div>
</template>

<script>
import { io } from "socket.io-client";

export default {
  props: ["room"],
  data() {
    return {
      socket: null,
      message: "",
      messages: [],
    };
  },
  mounted() {
    this.socket = io("http://localhost:5000");
    this.socket.emit("join_room", this.room);

    this.socket.on("receive_message", (data) => {
      this.messages.push(data);
    });
  },
  methods: {
    sendMessage() {
      if (this.message.trim() === "") return;
      const data = { room: this.room, message: this.message, sender: "You" };
      this.socket.emit("send_message", data);
      this.message = "";
    },
  },
  beforeDestroy() {
    this.socket.disconnect();
  },
};
</script>

<style>
.messages {
  border: 1px solid #ccc;
  padding: 10px;
  height: 300px;
  overflow-y: auto;
}
</style>

