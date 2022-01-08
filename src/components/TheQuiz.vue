<template>

  <div class="question-container">
    <h2 class="question">{{ questions[currentQuestionIndex]?.title }}</h2>
    <small class="question">{{ questions[currentQuestionIndex]?.id }}</small>
  </div>
  <div class="answer-container">
    <div v-for="{id, title} in answers" :key="id" class="answer-element">{{ title }}</div>
  </div>
  <button class="button-next-and-submit" @click="addToCurrentQuestionIndex">Next</button>
  <p>141 ir tema, 3913 ir jautajums, kas ir cits katras temas jaut</p>
</template>

<script lang="ts">
import axios from 'axios'
import {QuestionsAndAnswers} from '@/Types'

export default {
  name: "TheQuiz",
  props: {
    baseUrlAPI: String
  },
  data() {
    return {
      questions: [] as QuestionsAndAnswers[],
      answers: [] as QuestionsAndAnswers[],
      currentQuestionIndex: 0
    }
  },
  async mounted() {
    await axios
      .get(this.baseUrlAPI + 'questions&quizId=141')
      .then(({data}) => {
        this.questions = data
      })
      .catch((err) => console.log('Error catching questions', err));

    await axios
      // varbut japamaina sito qiestion INDEX uz ko citu, atkariba no initial choice category
      .get(this.baseUrlAPI + 'answers&quizId=141&questionId=' + this.questions[0]?.id)
      .then(({data}) => {
        this.answers = data
      })
      .catch((err) => console.log('Error catching answers', err))
    console.log(this.answers[0])
  },
  methods: {
    addToCurrentQuestionIndex() {
      if (this.currentQuestionIndex < this.questions.length - 1) {
        if (this.answers.length > 1) {
          this.currentQuestionIndex++
          axios
            .get(this.baseUrlAPI + 'answers&quizId=141&questionId=' + this.questions[this.currentQuestionIndex]?.id)
            .then(({data}) => {
              this.answers = data
            })
            .catch((err) => console.log('Error catching answers', err))
          console.log(this.answers[0])
        }
      }
    }
  },
  updated() {
    // axios
    //   .get(this.baseUrlAPI + 'answers&quizId=141&questionId=' + this.questions[this.currentQuestionIndex]?.id)
    //   .then(({data}) => {
    //     this.answers = data
    //   })
    //   .catch((err) => console.log('Error catching answers', err))
    // console.log(this.answers)
  }
//  if i know the category, do the api call
}
</script>

<style scoped>

</style>
