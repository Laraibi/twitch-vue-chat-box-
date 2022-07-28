<template>
  <div class="container">
    <div class="form" v-if="withForm">
      <label for="channelName">Twitch Channel :</label>
      <input type="text" v-model="channelName" class="form-control" />
      <button @click="subscribe">Subscribe</button>
    </div>
    <div class="leaderBoard" v-if="withForm">
      <table>
        <thead>
          <tr>
            <th>Rank</th>
            <th>Name</th>
            <th>Total</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(chatter, index) in bestThreeChatters.slice(0, 5)"
            :key="index"
          >
            <th>{{ index + 1 }}</th>
            <td>{{ chatter.name }}</td>
            <td>{{ chatter.count }}</td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="chatContainer" @resize="() => wconsole.log('resize')">
      <div
        class="chat w3-animate-right"
        v-for="(msg, index) in visiblesMessages"
        :key="index"
      >
        <span class="userName">{{ msg.user }}:</span>
        <p class="msgBody">{{ msg.message }}</p>
      </div>
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
      channelName: "larageeks",
      withForm: false,
    };
  },
  mounted() {
    ComfyJS.Init("larageeks");
    ComfyJS.onChat = this.receivedMSg;
  },
  methods: {
    receivedMSg(user, message, flags, self, extra) {
      let id = new Date().toString();
      this.messages.push({
        id,
        user,
        message,
        self,
        extra,
        flags,
        visible: true,
      });
      setTimeout(() => {
        let index = this.messages.findIndex((msg) => msg.id === id);
        this.messages[index].visible = false;
      }, 3000);
    },
    subscribe() {
      ComfyJS.Init(this.channelName);
    },
  },
  computed: {
    visiblesMessages() {
      return this.messages.filter((msg) => msg.visible);
    },
    chattersCounts() {
      return this.messages.reduce((counts, msg) => {
        // if (!counts.hasOwnProperty(msg.user)) {
        if (!Object.prototype.hasOwnProperty.call(counts, msg.user)) {
          counts[msg.user] = 0;
        }
        counts[msg.user]++;
        return counts;
      }, {});
    },
    bestThreeChatters() {
      let chattersNames = Object.keys(this.chattersCounts);
      let ArrayCount = chattersNames.map((name) => {
        return {
          name,
          count: this.chattersCounts[name],
        };
      });
      return ArrayCount.sort((a, b) => b.count - a.count);
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
  position: relative;
  background: linear-gradient(-81deg, #3b3b3b 0%, #34e4ff 100%);
  color: #ffffff;
  font-family: Arial;
  font-size: 20px;
  /* line-height: 120px; */
  /* text-align: center; */
  width: 100%;
  /* height: 120px; */
  border-radius: 14px;
  padding: 15px;
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
}
.chat:after {
  content: "";
  position: absolute;
  display: block;
  width: 0;
  z-index: 1;
  border-style: solid;
  border-width: 19px 0 0 19px;
  border-color: transparent transparent transparent #3b3b3b;
  top: 73%;
  right: -19px;
  margin-top: -9.5px;
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
