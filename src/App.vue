<template>
  <div class="chatContainer">
    <div class="chat" v-for="(msg, index) in messages" :key="index">
      <span class="userName">{{ msg.user }}:</span>
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
      this.messages.push({ user, message, self, extra, flags });
      setTimeout(() => {
        this.messages = this.messages.filter((msg) => msg.user != user);
      }, 3000);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Righteous&display=swap");
* {
  padding: 0;
  margin: 0;
}
.chat {
  background-color: rgb(52, 228, 255);
  color: white;
  flex-direction: row;
  align-items: center;
  padding: 10px;
  font-size: 2em;
  margin: 5px;
  font-family: "Righteous", cursive;
  border: solid 0.5px black;
  border-radius: 15px;
}
.userName {
  color: black !important;
  font-weight: bold;
  margin-right: 15px;
}
.msgBody {
  text-align: end;
}
</style>
