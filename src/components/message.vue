<script setup>
import { ref } from 'vue'
import {query_header, query_content, query_footer, query_header_intro} from './prompts.vue'

// do not use require, but use import
import Groq from 'groq-sdk';


const groq = new Groq({
  apiKey: import.meta.env.VITE_API_KEY,
  dangerouslyAllowBrowser: true
});


const question_string = ref("");
var message = ref("");

async function getNewResponse() {
  const full_query = query_header.concat("\n", query_content, question_string.value, "\n", query_footer)// question_string.value
  const chatCompletion = await groq.chat.completions.create({
     //query_header.concat("\n", question_string.value, "\n", query_footer)
    messages: [{ role: 'user', content: full_query }],
    model: 'llama3-8b-8192',
  });

  //console.log(chatCompletion.choices[0].message.content);
  return chatCompletion.choices[0].message.content
  //return "LLM stands for Low Latency Logistic Management, which refers to the process of managing and optimizing the movement of goods and materials with a focus on reducing the time it takes for these items to move through the supply chain. Low latency is important in logistics management for several reasons: 1. Improved customer satisfaction: In today's fast-paced business environment, customers expect their orders to be delivered quickly and accurately. Low latency LLMs help ensure that orders are fulfilled on time, which can lead to increased customer satisfaction and loyalty. 2. Increased efficiency: Low latency LLMs help reduce the time it takes for goods and materials to move through the supply chain, which can lead to increased efficiency and productivity. This is because goods and materials are available for use or sale more quickly, which can reduce the amount of time and resources needed to manage inventory. 3."
}


async function getIntro() {
  const full_query = query_header_intro.concat("\n", query_content, question_string.value, "\n", query_footer)// question_string.value
  const chatCompletion = await groq.chat.completions.create({
     //query_header.concat("\n", question_string.value, "\n", query_footer)
    messages: [{ role: 'user', content: full_query }],
    model: 'llama3-8b-8192',
  });

  //console.log(chatCompletion.choices[0].message.content);
  return chatCompletion.choices[0].message.content
  //return "LLM stands for Low Latency Logistic Management, which refers to the process of managing and optimizing the movement of goods and materials with a focus on reducing the time it takes for these items to move through the supply chain. Low latency is important in logistics management for several reasons: 1. Improved customer satisfaction: In today's fast-paced business environment, customers expect their orders to be delivered quickly and accurately. Low latency LLMs help ensure that orders are fulfilled on time, which can lead to increased customer satisfaction and loyalty. 2. Increased efficiency: Low latency LLMs help reduce the time it takes for goods and materials to move through the supply chain, which can lead to increased efficiency and productivity. This is because goods and materials are available for use or sale more quickly, which can reduce the amount of time and resources needed to manage inventory. 3."
}

const first_message = await getIntro();


async function setM (){
    
    message.value = await getNewResponse();
    console.log("setM ran")
}

</script>

<template>
    
    <h4>{{ first_message }}</h4>

    <div>
    <input type="text" placeholder="Enter your question here" v-model="question_string" >
    
    <button @click="setM">Go!</button>
    </div>
    <p>{{ message }}</p>



</template>