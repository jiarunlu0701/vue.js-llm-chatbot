<template>
  <div>
    <!-- User Title -->
    <div class="title user-title">You</div>

    <!-- Chat Container -->
    <div class="chat-container">
      <!-- Chat Messages -->
      <div v-for="(message, index) in chatMessages" :key="index" :class="message.role">
        {{ message.content }}
      </div>

      <!-- Response Message -->
      <div v-if="responseContent" class="assistant">
        {{ responseContent }}
      </div>
    </div>

    <!-- Chatbot Title -->
    <div class="title chatbot-title">Chatbot</div>

    <!-- Input Box -->
    <div class="input-container">
      <input v-model="userMessage" @keyup.enter="sendMessage" type="text" placeholder="Type your message...">
      <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userMessage: '',
      chatMessages: [],
      responseContent: null
    };
  },
  methods: {
    async sendMessage() {
      if (this.userMessage) {
        // Add user message to chat
        this.chatMessages.push({ role: 'user', content: this.userMessage });

        try {
          // Send user message to the API
          const res = await axios.post('http://39.106.152.134:8000/v1/chat/completions', {
            model: "/root/.cache/modelscope/hub/AI-ModelScope/TinyLlama-1.1B-Chat-v1.0",
            messages: [
              { role: "system", content: "You are a helpful assistant." },
              { role: "user", content: this.userMessage }
            ]
          }, {
            headers: {
              'Content-Type': 'application/json'
            }
          });

          // Add assistant's response to chat
          this.chatMessages.push({ role: 'assistant', content: res.data.choices[0].message.content });

        } catch (error) {
          console.error(error);
          this.chatMessages.push({ role: 'assistant', content: 'Error occurred: ' + error.message });
        }

        // Clear the input box
        this.userMessage = '';
      }
    }
  }
}
</script>

<style scoped>
/* Add some styles for the chat interface */
.title {
  font-weight: bold;
  text-transform: uppercase;
  margin: 10px 0;
}

.user-title {
  text-align: right;
  color: #007BFF;
}

.chatbot-title {
  text-align: left;
  color: #f1f1f1;
}

.chat-container {
  max-width: 400px;
  margin: 0 auto;
}

.user {
  text-align: right;
  background-color: #007BFF;
  color: #fff;
  border-radius: 10px;
  padding: 10px;
  margin: 5px;
}

.assistant {
  text-align: left;
  background-color: #f1f1f1;
  color: #333;
  border-radius: 10px;
  padding: 10px;
  margin: 5px;
}

.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  background-color: #007BFF;
  color: #fff;
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  margin-left: 10px;
  cursor: pointer;
}
</style>
