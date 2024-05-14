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

  return chatCompletion.choices[0].message.content
//   return "<example answer to your question.>" + question_string.value
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
//   return "<Hi I am example Jay!>"
}

const first_message = await getIntro();


async function setM (){
    
    message.value = await getNewResponse();
    console.log("setM ran")
}

</script>

<template>

<div class="p-5 text-center">
  <h1>JayBot Virtual Assistant</h1>
  <p>Using Retrieval-augmented generation</p>
  <p>Answers to questions pertaining to <a href="https://www.jrsmith.com/uploads/fileLibrary/PG-24-Terms-and-Conditions.pdf">Jay R. Smith T&C PDF.</a></p> 
</div>

<div class="container-sm" style="max-width: 600px;">
    <div class="row ">
        <p>{{ first_message }}</p>
    </div>


    <div class="row">
        <div class="col-sm-10">
            <input class="form-control" type="text" placeholder="Enter your question here" v-model="question_string" >
        </div>   
        <div class="col-sm-1">
            <button class="btn btn-primary" @click="setM">Go!</button>
            <p></p>
        </div>   
    </div>

    <div class="row">
        <h6>Answers:</h6>
    </div>       
    <div class="row">
        <div class="col">
            <p>{{ message }}</p>
        </div>
    </div>
    <div class="row m-5">
      <p></p>
    </div>

 </div>




</template>