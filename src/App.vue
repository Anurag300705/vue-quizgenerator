<script setup>

import { GoogleGenAI, Type } from "@google/genai";
import StartScreen from './components/StartScreen.vue';
import Quiz from "./components/Quiz.vue";
import Loader from "./components/Loader.vue";
import Result from "./components/Result.vue";
import { ref } from 'vue';

const API_KEY = import.meta.env.VITE_API_KEY; // Use the API key from the environment variable

const questions = ref('');
const status = ref('start'); // This can be used to track the status of the quiz generation 
// Also is Declarative UI - Screen that will be shown depends on status of the application
const userAnswers = ref([]);  // Array to store user's answers

const storeAnswer = (answer) => {
  userAnswers.value.push(answer);  // Store the user's answer in the userAnswers array
}

const startQuiz = async (topic) => {

  status.value = 'loading'; // Change status to loading while fetching questions
  const ai = new GoogleGenAI({ apiKey: API_KEY });

  async function main() {
    const response = await ai.models.generateContent({
      model: "gemini-2.5-flash",
      contents:
        `Create 5 quiz questions about ${topic}
        Difficulty level: easy to medium
        Type: multiple choice
        `,
      config: {
        responseMimeType: "application/json",
        responseSchema: {
          type: Type.OBJECT,
          properties: {
            response_code: {
              type: Type.NUMBER,
            },
            results: {
              type: Type.ARRAY,
              items: {
                type: Type.OBJECT,
                properties: {
                  type: {
                    type: Type.STRING,
                  },
                  difficulty: {
                    type: Type.STRING,
                  },
                  category: {
                    type: Type.STRING,
                  },
                  question: {
                    type: Type.STRING,
                  },
                  correct_answer: {
                    type: Type.STRING,
                  },
                  incorrect_answers: {
                    type: Type.ARRAY,
                    items: {
                      type: Type.STRING,
                    },
                  },
                },
                propertyOrdering: [
                  "type",
                  "difficulty",
                  "category",
                  "question",
                  "correct_answer",
                  "incorrect_answers"
                ],
              },
            },
          },
          propertyOrdering: ["response_code", "results"],
        },
      }
      ,
    });

    questions.value = JSON.parse(response.text);
    status.value = 'ready'; // Change status to ready after fetching questions
      // console.log(questions.value);
  }

  main();

};

</script>

<template>
  <div id="app">

    <header>
      <div class="container">
        <img src="./assets/logo.png" alt="" class="logo">
        <h1>Quiz Generator</h1>
      </div>
    </header>

    <StartScreen v-if="status == 'start'" @start-quiz="startQuiz" />
    <Loader v-if="status == 'loading'" />
    <Quiz v-if="status == 'ready'" @end-quiz="status = 'finished'" @store-answer="storeAnswer" :questions="questions.results"  />
    <Result v-if="status == 'finished'" :userAnswers="userAnswers" />
  </div>
</template>
