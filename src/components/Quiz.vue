<script setup>

const props = defineProps(['questions']);
const emit = defineEmits(['store-answer', 'end-quiz']);

import { ref, computed } from 'vue';

const currentQuestion = ref(0);
const selectedOption = ref(null);

const shuffledOptions = computed(() => {
    let options = [...props.questions[currentQuestion.value].incorrect_answers];  // Create a copy of the incorrect answers
    options.splice(Math.round(Math.random() * options.length), 0, props.questions[currentQuestion.value].correct_answer); // Add the correct answer at a random position
    return options;
})

const submitAnswer = () => {
    emit('store-answer', {
        question: props.questions[currentQuestion.value],
        userAnswer: selectedOption.value,
    });
    selectedOption.value = null; // Reset selected option for the next question

    if (currentQuestion.value === props.questions.length - 1) {
        emit('end-quiz'); // Emit event when quiz is completed
    } else{
        currentQuestion.value += 1;
    }
}


</script>

<template>
    <section class="quiz container">

        <div class="header">
            <h2>Quiz</h2>
            <p>Question {{ currentQuestion + 1 }} of {{ props.questions.length }}</p>
        </div>

        <progress max="100" :value="((currentQuestion + 1) / props.questions.length) * 100" />

        <div class="question">
            <h2>
                {{ props.questions[currentQuestion].question }}
            </h2>
        </div>

        <div class="answers">
            <button class="answer" :class="{ active: answer === selectedOption }" v-for="answer in shuffledOptions"
                :key="answer" @click="selectedOption = answer">
                {{ answer }}
            </button>
        </div>

        <button v-if="selectedOption" @click="submitAnswer">Send</button>


    </section>
</template>