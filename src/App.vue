<template>
  <main class="app">
    <h1>The Quiz</h1>
    <section class="quiz" v-if="!quizCompleted">

      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span>Score {{ score }} / {{ questions.length }}</span>
      </div>

        <div class="options">
          <label v-for="(option,index) in getCurrentQuestion.options "
          :key="index"
          :class="`option ${getCurrentQuestion.selected == index 
          ? index == getCurrentQuestion.answer
          ? 'correct' :  'wrong' :''
        } 
        ${getCurrentQuestion.selected != null && 
        index != getCurrentQuestion.selected ?
        'disabled' :''
        }
          `"
          >
            <input 
            type="radio" 
            :name="getCurrentQuestion.index" 
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="setAnswer"
            >
            <span>{{ option }}</span>

          </label>
        </div>
        <button 
        @click="nextQuestion"
        :disabled="!getCurrentQuestion.selected"
        >
        {{ getCurrentQuestion.index == questions.length -1 ? 
        'Finish'  
        : getCurrentQuestion.selected == null ?
        'Select an option' :
        'Next option'
      }}
      </button>

    </section>

    <section v-else>
      <h2>You have the fnished quiz!</h2>
      <p>Your score {{ score }} / {{ questions.length }}</p>

    </section>
  </main>
</template>

<script setup>
import { ref, computed } from 'vue'


const questions = ref([
  {
    question: 'What is Vue JS ?',
    answer: 0,
    options: [
      'A front end framework',
      'A library',
      'An ice cream maker'
    ],
    selected: null,
  },
  {
    question: 'What is Vuex ?',
    answer: 2,
    options: [
      'Vue with and x',
      'A cheese selection',
      'State managment library'
    ],
    selected: null,
  },
  {
    question: 'What is Vue Router used for?',
    answer: 1,
    options: [
      'Walking in space',
      'A routing library for Vue JS',
      'Burger sauce',
      'Quizzes'
    ],
    selected: null,
  },
  {
    question: 'What is Bootstrap css?',
    answer: 3,
    options: [
      'A routing library for Vue JS',
      'Walking in space',
      'State Menagment library',
      'CSS library'
    ],
    selected: null,
  },
  {
    question: 'What is pinia?',
    answer: 1,
    options: [
      'UI/UX library',
      'State Menagment library for Vue',
      'Javascript framework',
      'State Menagment library for React',
    ],
    selected: null,
  },
])
const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
  let value = 0
  questions.value.map(q => {
    if (q.selected == q.answer) {
      value++
    }
  })
  return value
})

const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value]
  question.index = currentQuestion.value
  return question
})
const setAnswer = e => {
  questions.value[currentQuestion.value].selected = e.target.value
  e.target.value = null
}
const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted.value = true
  }
}

</script>

<style >
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  background-color: #271c36;
  color: #fff;
}
.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}
h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}
.quiz{
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}
.quiz-info{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}
.quiz-info .question{
  color: #8F8F8F;
  font-size: 1.25rem;
}
.quiz-info .score{
  color: #fff;
  font-size: 1.25rem;
}
.options{
  margin-bottom: 1rem;
}
.option{
  display: block;
  padding: 1rem;
  background-color: #271C36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}
.option:hover{
  background-color: #2d213f;
}
.option.correct{
  background-color: #2cce7d;
}
.option.wrong{
  background-color: #ff5a5f;
}
.option.disabled{
  opacity: 0.5;
}
.option input{
  display: none;
}
button{
  appearance: none;
  border: none;
  outline: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: #2cce7d;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
  color: #2d213f;
}
button:disabled{
  opacity: 0.5;
}
h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p{
  color: #8f8f8f;
  font-size: 1.25rem;
  text-align: center;
}
</style>
