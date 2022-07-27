<template>
  <div class="chatContainer">
    <div class="chat" v-for="(msg, index) in messages" :key="index">
      <span class="userName">{{ msg.user }}</span>
      <p class="msgBody">{{ msg.message }}</p>
    </div>
  </div>
</template>

<script>
import ComfyJS from "comfy.js";
export default {
  name: "App",
  data: () => {
    return {
      messages: [],
      ComfyJS: ComfyJS,
    };
  },
  mounted() {
    ComfyJS.Init("larageeks");
    ComfyJS.onChat = this.receivedMSg;
  },
  methods: {
    receivedMSg(user, message, flags, self, extra) {
      console.log("msg received");
      this.messages.push({ user, message, self, extra, flags });
    },
  },
};
</script>

<style>
.chat {
  background-color: rgba(41, 130, 255, 0.4);
  color: white;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 5px 10px;
  font-size: 2em;
}
.userName{
  color:black !important;
  font-weight: bold;
  margin-right: 15px;
}
</style>
