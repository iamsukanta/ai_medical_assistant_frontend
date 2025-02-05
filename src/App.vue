<script setup>
import axios from "axios";
import { ref } from "vue";
const query = ref("");
const searchQuery = ref("");
const response = ref("");

const askAssistant = async() => {
  if (!query.value) return;
    try {
      const res = await axios.post("http://127.0.0.1:5000/search", { query: query.value });
      if(res.data?.matches?.query) {
        response.value = res.data?.matches?.response[0]?.generated_text;
      } else {
        response.value = res.data?.response[0]?.generated_text;
      }
    } catch (error) {
      console.error("Error:", error);
    }
}

const searchText = async() => {
  if (!searchQuery.value) return;
    try {
      const res = await axios.post("http://127.0.0.1:5000/search", { query: searchQuery.value });
      response.value = res.data?.response[0]?.generated_text;
    } catch (error) {
      console.error("Error:", error);
    }
}


</script>

<template>
  <div class="container">
    <h1>Medical Assistant</h1>
    <textarea v-model="query" placeholder="Ask a medical question..."></textarea>
    <button @click="askAssistant">Ask</button>

    <div class="flex">
      <input type="text" v-model="searchQuery" placeholder="Search ..." />
      <button class="btn" type="button" @click="searchText()">Search</button>
    </div>

    <br/>
    <p v-if="response">
      <span v-html="response"></span>
    </p>
  </div>


</template>


<style>
.container {
  max-width: 600px;
  margin: auto;
  text-align: center;
}
textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
}
button {
  padding: 10px;
  background: blue;
  color: white;
  border: none;
}

.flex {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 10px;
  margin-top: 20px;
}
</style>
