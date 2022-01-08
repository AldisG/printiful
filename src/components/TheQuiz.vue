<template>
  <div class="quiz-container" v-if="answers.length > 1">
    <div class="question-container">
      <h2 class="question">{{ questions[currentQuestionIndex]?.title }}</h2>
      <small class="question">{{ questions[currentQuestionIndex]?.id }}</small>
    </div>

    <div class="answer-container" v-if="!(currentQuestionIndex === questions.length)">
      <div v-for="{id, title} in answers" :key="id" class="answer-element">{{ title }}</div>
    </div>

    <span>Answered: {{ currentQuestionIndex}}/{{ questions.length }}</span>

    <button
      v-if="(currentQuestionIndex <= questions.length - 1)"
      class="button-next-and-submit" @click="addToCurrentQuestionIndex">
      Submit answer
    </button>

    <div v-if="(currentQuestionIndex === questions.length)">
<!--      šim jāpadod arājs ar ID array un jāmapo uz vienu garu string, ko sūta submit axios-->
      <QuizComplete :baseUrlAPI="baseUrlAPI" />
      <button class="button-next-and-submit" @click="finishQuiz">
        See Results!
      </button>
    </div>
    <small>141 ir tema, 3913 ir jautajums, kas ir cits katras temas jaut</small>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import {QuestionsAndAnswers} from '@/Types'
import QuizComplete from "@/components/QuizComplete.vue";

export default {
  name: "TheQuiz",
  props: {
    baseUrlAPI: String
  },

  data() {
    return {
      currentQuestionId: 0,
      questions: [] as QuestionsAndAnswers[],
      answers: [] as QuestionsAndAnswers[],
      currentQuestionIndex: 0,
      userPoints: 0,
      quizComplete: false
    }
  },

  components: {
    QuizComplete
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
  },
  methods: {
    addToCurrentQuestionIndex() {
      if (this.currentQuestionIndex === this.questions.length) {
        // console.log('Quiz complete', this.currentQuestionIndex, this.questions.length - 1)
        // console.table('Quiz : ', this.questions)
        this.quizComplete = true
        return
      }
      console.log(this.currentQuestionIndex + 1, 'Can COUNT USER points IF CORRECT')
      if (this.currentQuestionIndex < this.questions.length ) {
          this.currentQuestionIndex++
      }

      if (this.currentQuestionIndex <= this.questions.length - 1) {
        if (this.answers.length > 1) {
          axios
            .get(this.baseUrlAPI + 'answers&quizId=141&questionId=' + this.questions[this.currentQuestionIndex]?.id)
            .then(({data}) => {
              this.answers = data
            })
            .catch((err) => console.log('Error catching answers', err))
        }
      }
    },
    finishQuiz() {
      console.log('Quiz finished, show modal or page content')
    }
  },
}
</script>

<style scoped>

</style>
