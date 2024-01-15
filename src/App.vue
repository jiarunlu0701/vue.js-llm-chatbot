<template>
  <div id="app">
    <div id="llama-page">
      <h1>Welcome to the Page!</h1>
    </div>

    <!-- Include ChatComponent here -->
    <ChatComponent />
  </div>
</template>

<script>
import ChatComponent from './components/ChatComponent.vue'; //
export default {
  name: 'App',
  components: {
    ChatComponent // Register the ChatComponent
  }
};
</script>

