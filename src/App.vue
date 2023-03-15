<script setup>
import {ref , computed } from 'vue'

const questions = ref([
  {
    question : 'What is Vue JS ?',
    answer : 0 ,
    options : [
      'A front end framework' ,
      'A back end framework' ,
      'A full stack framework' ,
      'A mobile framework'
    ],
    selected : null
  },
  {
    question : 'What is Vuex?',
    answer :  2,
    options : [
      'Vue with an x ',
      'A cheese selection',
      'State management library',
      'A state management library for Vue'
    ],
    selected : null
  },
  {
    question : 'What is Vue Router used for?',
    answer :  1,
    options : [
     'walking in space',
      'A router for Vue',
      'Burger sauce',
      'Burger sauce for Vue'
    ],
    selected : null
  }
])

const currentQuestion = ref(0)
const quizCompleted = ref(false)
const Score = computed(() => {
  let value = 0
  questions.value.map((question) => {
    if (question.selected == question.answer) {
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

const SetAnswer = evt => {
  console.log(questions);
  questions.value[currentQuestion.value].selected = evt.target.value
  evt.target.value = null 
}

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++
  } else {
    quizCompleted.value = true
  }
}


</script>

<template>
<main class="app">
  <div  id="quiz">
        <div class="quiz-header">
          <h2>Quiz</h2>
          <div id="timer">
            <h2 id="time">00</h2>
          </div>
        </div>
        
      <div  id="question">
        <div class="quiz-info">
    <h2 class="question">{{ getCurrentQuestion.question }}</h2>
    </div>
          </div>
          <div onclick="" id="questionText">
            <label v-for="(option, index) in getCurrentQuestion.options" :key="index"
    :class="`option ${
getCurrentQuestion.selected == index 
? index == getCurrentQuestion.answer
? 'correct'
: 'incorrect'
: ''
    }${
      getCurrentQuestion.selected != null && index != getCurrentQuestion.selected 
      ? 'disabled'
      : ''
    }`">


      <input type="radio" :name="getCurrentQuestion.index" :value="index" v-model="getCurrentQuestion.selected" :disabled="getCurrentQuestion.selected" @change="SetAnswer"/>
      <span>{{ option }}</span>
    </label>
          </div>
     </div>

  <section class="quiz">
  

  <div class="options">
   
  </div>
  <button
    @click="NextQuestion"
    :disabled="!getCurrentQuestion.selected">
    {{ 
      getCurrentQuestion.index < questions.length - 1
      ? 'next question'
      : getCurrentQuestion.selected == null
      ? 'select an option'
      : 'finish quiz'
    }}
  </button>
    </section>
    <section class="results" v-if="quizCompleted">
      <h2>Quiz Completed</h2>
      <p>You scored {{ Score }}/{{ questions.length }}</p>
    </section>
    </main>

</template>

<style >
* {
   margin : 0;
    padding : 0;
    box-sizing : border-box;
    font-family : 'Montserrat', sans-serif;
}
  body{
    background-color : #271C36;
    color : #fff;
 }
</style>
