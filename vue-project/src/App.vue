<template>
    <input type="text" v-model="page.title">
    Temps écoulé : {{ time }}
    <button @click="reset">Reset</button>
</template>

<script setup>
import { ref, watch, watchEffect } from 'vue'
import { userTimer } from './composable/newTimer';
const page = ref({
    title: ''
})
const {time, reset} = userTimer()
watchEffect(()=> 
document.title = page.value.title)
</script>