<template>
  <div class="quiz-container">
    <div class="question-container">
      <h2 class="question">{{ questions[currentQuestionIndex]?.title }}</h2>
      <small class="question">{{ questions[currentQuestionIndex]?.id }}</small>
    </div>

    <div class="answer-container" v-if="!(currentQuestionIndex === questions.length)">
      <button
        type="button"
        v-for="{id, title} in answers"
        :key="id"
        @click="addToCurrentQuestionIndex(e,id)"
        class="answer-element">{{ title }}
      </button>
    </div>

    <span>Answered: {{ currentQuestionIndex }}/{{ questions.length }}</span>

<!--    <button-->
<!--      v-if="(currentQuestionIndex <= questions.length - 1)"-->
<!--      class="button-next-and-submit" @click="addToCurrentQuestionIndex">-->
<!--      Submit answer-->
<!--    </button>-->

    <div v-if="(currentQuestionIndex === questions.length)">
      <QuizComplete
        :baseUrlAPI="baseUrlAPI"
        :userAnswerListIntoString="userAnswerListIntoString"
        :userChoice="userChoice" />
<!--      <button class="button-next-and-submit" @click="finishQuiz">-->
<!--        See Results!-->
<!--      </button>-->
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import {QuestionsAndAnswers} from '@/Types'
import {UserChoice} from '@/Types.ts'

import QuizComplete from "@/components/QuizComplete.vue";

export default {
  name: "TheQuiz",
  props: {
    baseUrlAPI: String,
    userChoice: {} as UserChoice,
  },

  data() {
    return {
      questions: [] as QuestionsAndAnswers[],
      answers: [] as QuestionsAndAnswers[],
      currentQuestionId: 0,
      questionID: '',
      currentQuestionIndex: 0,
      userAnswerListIntoString: '',
      quizComplete: false
    }
  },

  components: {
    QuizComplete
  },

  async mounted() {
    this.questionID = this.userChoice.selectedID
    console.log('success', this.userChoice)
    await axios
      .get(this.baseUrlAPI + 'questions&quizId=' + this.questionID)
      .then(({data}) => {
        this.questions = data
      })
      .catch((err) => console.log('Error catching questions', err));

    await axios
      .get(`${this.baseUrlAPI}answers&quizId=${this.questionID}&questionId=${this.questions[0]?.id}`)
      .then(({data}) => {
        this.answers = data
        console.log('current querst', this.answers.length)

      })
      .catch((err) => console.log('Error catching answers', err))
  },
  methods: {
    addToCurrentQuestionIndex(e, selectedID) {
      console.log('current querst', this.answers)
      if (this.currentQuestionIndex === this.questions.length) {
        this.quizComplete = true
        return
      }
      if (this.currentQuestionIndex < this.questions.length) {
        this.currentQuestionIndex++
        this.userAnswerListIntoString += `&answers[]=${selectedID}`
      }

      if (this.currentQuestionIndex <= this.questions.length - 1) {
        if (this.answers.length > 0) {
          axios
            .get(`${this.baseUrlAPI}answers&quizId=${this.questionID}&questionId=${this.questions[this.currentQuestionIndex]?.id}`)
            .then(({data}) => {
              this.answers = data
            })
            .catch((err) => console.log('Error catching answers', err))
        }
      }
    },
    // finishQuiz() {
    //   console.log('Quiz finished, show modal or page content')
    // }
  },
}
</script>

<style scoped>

</style>
