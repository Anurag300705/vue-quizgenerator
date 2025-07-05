<script setup>
const props = defineProps(['userAnswers']);
import { computed } from 'vue';

const correctAnswersCount = computed(() => {
    return props.userAnswers.filter(answer => answer.userAnswer === answer.question.correct_answer).length;
})
</script>

<template>
    <section class="result-screen container">
        <h1 v-if="correctAnswersCount == userAnswers.length">&#127881; Congratulations!</h1>
        <h1 v-else> &#128522; Better luck next time!</h1>
        <h1>You have answered {{ correctAnswersCount }} out of {{ props.userAnswers.length }} questions correctly</h1>

        <ul>
            <li v-for="(answer, index) in props.userAnswers" :key="index"
                :class="answer.question.correct_answer === answer.userAnswer ? 'correct' : 'incorrect'">
                <p v-html="answer.question.correct_answer == answer.userAnswer ? '&#9989;' : '&#10060;'"></p>
                <b> {{ answer.question.question }}</b>
                <p> Your answer: {{ answer.userAnswer }}</p>
                <p> Correct answer: {{ answer.question.correct_answer }}</p>
            </li>
        </ul>
    </section>
</template>