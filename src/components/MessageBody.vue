<script>
export default {
  data() {
    return {
      connection: null,
      messages: [],
      userMessage: '',
    }
  },

  methods: {
    sendMessage(message) {
      console.log("Sending message:", message);
      this.connection.send(message);
      this.userMessage = ''
    }
  },

  mounted() {
    console.log("Starting connection to WebSocket Server")
    this.connection = new WebSocket("ws://localhost:8001/ws")

    this.connection.onmessage = (event) => {
      console.log("Received message from WebSocket Server");
      let date = new Date()
      let nowTime = date.getHours().toString() + ':' + date.getMinutes().toString()
      this.messages.push({ text: event.data, time: nowTime });
    }

    this.connection.onopen = (event) => {
      console.log(event)
      console.log("Successfully connected to the echo websocket server...")
    }
  },
}
</script>

<template>
  <div class="bg-light bg-opacity-50" id="quote-box">
    <div>
      <p class="message" v-for="msg in messages">{{ msg.time }}: {{ msg.text }}</p>
      <br />
    </div>
    <div>
      <input v-model="userMessage" />
      <button class="btn btn-primary" id="btn-send" @click="sendMessage(userMessage)">Send</button>
    </div>
  </div>
</template>