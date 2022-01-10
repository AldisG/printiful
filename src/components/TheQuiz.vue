<template>
  <h1 v-if="!answers.length">Loading...</h1>

  <div v-if="!quizComplete" >
    <div v-if="answers.length" class="quiz-container">
      <div class="question-container">
        <h2 class="question">{{ questions[currentQuestionIndex]?.title }}</h2>
      </div>

        <span v-if="loadingNewAnswerList">Loading...</span>
        <div class="answer-container" v-if="!loadingNewAnswerList">
          <button
            type="button"
            v-for="{id, title} in answers"
            :key="id"
            @click="addToCurrentQuestionIndex(e,id)"
            class="answer-button">{{ title }}
          </button>
        </div>
        <span>Answered: <b>{{ currentQuestionIndex }}/{{ questions.length }}</b></span>
    </div>
  </div>

    <div v-if="quizComplete">
      <QuizComplete
        :baseUrlAPI="baseUrlAPI"
        :userAnswerListIntoString="userAnswerListIntoString"
        :userChoice="userChoice"
        @reactivateQuizUp="quizCompleteReset"/>
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

  components: {
    QuizComplete
  },

  data() {
    return {
      questions: [] as QuestionsAndAnswers[],
      answers: [] as QuestionsAndAnswers[],
      currentQuestionId: 0,
      questionID: '',
      currentQuestionIndex: 0,
      userAnswerListIntoString: '',
      loadingNewAnswerList: false,
      quizComplete: false,
    }
  },

  async mounted() {
    this.questionID = this.userChoice.selectedID
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
      })
      .catch((err) => console.log('Error catching answers', err))
  },
  methods: {
    quizCompleteReset() {
      this.$emit('reactivateQuizUpToHome')
    },
    addToCurrentQuestionIndex(e, selectedID) {
      if (this.currentQuestionIndex === this.questions.length - 1) {
        this.quizComplete = true
        this.loadingNewAnswerList = true
        return
      }
      if (this.currentQuestionIndex < this.questions.length) {
        this.currentQuestionIndex++
        this.userAnswerListIntoString += `&answers[]=${selectedID}`
        this.loadingNewAnswerList = true
      }

      if (this.currentQuestionIndex <= this.questions.length - 1) {
        if (this.answers.length > 0) {
          axios
            .get(`${this.baseUrlAPI}answers&quizId=${this.questionID}&questionId=${this.questions[this.currentQuestionIndex]?.id}`)
            .then(({data}) => {
              this.answers = data
              this.loadingNewAnswerList = false
            })
            .catch((err) => {
              console.log('Error catching answers', err)
              this.loadingNewAnswerList = false
            })
        }
      }
    },
  },
}
</script>

<style scoped>
  .quiz-container{
    min-height: 350px;
  }
  .quiz-container,
  .answer-container {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 30px;
  }
  .answer-container{
    gap: 10px;
  }
</style>
