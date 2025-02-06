<script setup>
import axios from "axios";
import { ref } from "vue";
import OverlayLoader from "../components/common/OverlayLoader.vue";
const query = ref("");
const response = ref("");
let isLoading = ref(false);

const askAssistant = async() => {
  if (!query.value) return;
    try {
      isLoading.value = true;
      const res = await axios.post(`${import.meta.env.VITE_API_BASE_URL}/search`, { query: query.value });
      if(res.data?.matches?.query) {
        response.value = res.data?.matches?.response[0]?.generated_text;
      } else {
        response.value = res.data?.response[0]?.generated_text;
      }
      isLoading.value = false;
    } catch (error) {
      console.error("Error:", error);
      isLoading.value = false;
    }
}

const searchByQuestion = (value) => {
    query.value = value;
    askAssistant();
}

function decodeHtml(html) {
    const txt = document.createElement("textarea");
    txt.innerHTML = html;
    return txt.value;
}

const formatedResponse = (value) => {
    let extractValue =  value.replace(/^[^\n]*\n/, '');
    let decodeValue = decodeHtml(extractValue);
    return decodeValue;
}
</script>

<template>
    <OverlayLoader :loadingValue="isLoading" />
    <div class="flex flex-col justify-center items-center px-12 py-7">
        <h2 class="text-3xl text-gray-500">Sample Medical Assistant</h2>
        <p class="text-gray-500">Built with Python Flask, Vue 3, LLM model using Huggingface API, MongoDB database, Vector Database </p>
    </div>

    <div class="flex flex-col md:flex-row gap-4 justify-center items-center max-w-[900px] mx-auto">
        <button class="px-3 py-1 mt-1 mb-1 bg-[#EAEAEA] rounded-full" @click="searchByQuestion('How to keep fit?')">How to keep fit?</button>
        <button class="px-3 py-1 mt-1 mb-1 bg-[#EAEAEA] rounded-full" @click="searchByQuestion('How to protect myself from viral feaver?')">How to protect myself from viral feaver?</button>
    </div>
    <div class="flex flex-col md:flex-row gap-4 justify-center items-center max-w-[900px] mx-auto mt-2">
        <button class="px-3 py-1 mt-1 mb-1 bg-[#EAEAEA] rounded-full" @click="searchByQuestion('Why sometimes brain does not work?')">Why sometimes brain does not work?</button>
        <button class="px-3 py-1 mt-1 mb-1 bg-[#EAEAEA] rounded-full" @click="searchByQuestion('What is the symtom of Covid virus?')">What is the symtom of Covid virus?</button>
        <button class="px-3 py-1 mt-1 mb-1 bg-[#EAEAEA] rounded-full" @click="searchByQuestion('Good diet food for night meal.')">Good diet food for night meal.</button>
    </div>

    <div class="flex flex-col md:flex-row gap-5 justify-center flex-initial mt-7 px-7">
        <div class="">
            <textarea placeholder="Ask your query ..." v-model="query" class="border px-3 py-3 w-full md:w-[550px] lg:w-[600px] max-w-[800px]" rows="5" />
        </div>
        <div>
            <button class="px-10 py-2 rounded-full bg-blue-600 text-white hover:bg-blue-400 w-full md:w-auto" type="button">
                Search
            </button>
        </div>

    </div>

    <div v-if="response" class="flex flex-col mx-auto px-6 py-7 mt-3 max-w-[800px]">
        <p class="font-semibold text-gray-500">Your Question: {{ query }}</p>
        <b class="text-gray-700 mt-2">Answer: </b>
        <div class="text-gray-600 mt-3" v-html="formatedResponse(response)"></div>
    </div>
</template>