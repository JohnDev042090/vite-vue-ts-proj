<template>
    <div>
      <h2>Child Component</h2>
      <button @click="sendMessage">Send Message to Parent</button>
    </div>
  </template>
  
  <script lang="ts">
  export default {
    methods: {
      sendMessage() {
        const message = 'Hello from Child!';
        this.$emit('messageUpdated', message);
      }
    }
  };
  </script>
  