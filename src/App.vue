<template>
  <div id="app">
    <Container>
      <ChatWindow
        :text="text"
        :username="username"
        @send-data="this.sendMessage"
      >
        <div v-for="item in messages" :key="item.id">
          <ChatMessage :username="item.author" :time="item.datetime"
            >{{ item.text }}
          </ChatMessage>
        </div>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import ChatWindow from "./components/ChatWindow.vue";
import ChatMessage from "./components/ChatMessage.vue";

export default {
  name: "App",
  components: {
    Container,
    ChatWindow,
    ChatMessage,
  },
  data: () => ({
    messages: [],
  }),
  mounted() {
    this.loadMessage();
  },
  methods: {
    loadMessage() {
      this.axios
        .get("https://61bcd385d8542f0017824a2a.mockapi.io/messages")
        .then((response) => {
          this.messages = response.data;
        });
    },
    sendMessage(data) {
      console.log("send-data", data);
      this.axios
        .post("https://61bcd385d8542f0017824a2a.mockapi.io/messages", {
          author: data.username,
          text: data.text,
        })
        .then((response) => {
          console.log(response);
          this.loadMessage();
        });
    },
  },
};
</script>

<style>
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>
