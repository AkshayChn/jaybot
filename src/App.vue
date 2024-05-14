<!--
Main App of the JayBot!
-->

<script setup>
import { ref } from 'vue'
import message from './components/message.vue'
</script>

<template>
 
  <Suspense>
    <message/>
  </Suspense>

</template>