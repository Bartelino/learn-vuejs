<template>
    <div class="question">
        <h3>{{ question.question }}</h3>
        <ul>
            <li v-for="(choice, index) in randomChoices" :key="choice">
                <Answer :id="`answer${index}`" :disabled="hasAnswer" :value="choice"
                    :correctAnswer="question.correct_answer" @change="onAnswer" v-model="answer" />
            </li>
        </ul>
    </div>
</template>

<script setup>
import { shuffleArray } from '@/function/array';
import { computed, onMounted, onUnmounted, ref } from 'vue';
import Answer from './answer.vue';

const props = defineProps({
    question: Object
})
let timer
const answer = ref(null)
const emits = defineEmits(['answer'])
const hasAnswer = computed(() => answer.value !== null)
const onAnswer = () => {
    clearTimeout(timer)
    timer = setTimeout(() => {
        emits('answer', answer.value)
    }, 1_000)
}

const randomChoices = computed(() => shuffleArray(props.question.choices))


onMounted(() => {
    timer = setTimeout(() => {
        answer.value = ''
        onAnswer()
    }, 3_000)
})

onUnmounted(() => {
    clearTimeout(timer)
})
</script>

<style>
.question {
    padding-top: 2rem;
}

.question button {
    margin-left: auto;
    display: block;
}
</style>