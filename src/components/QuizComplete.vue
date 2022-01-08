<template>
  <div class="wrapper">
    <h2 class="quiz-complete">Quiz completed!</h2>
    <h4 class="user">Thanks "" NAME ""!</h4>
    <span v-if="score" class="user-final-score">{{ score.correct }} / {{score.total}}.</span>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import { AnswerResults } from "@/Types";

export default ({
  name: "QuizComplete",
  props: {
    baseUrlAPI: String
  },
  data() {
    return {
      score: {
        correct: 0,
        total: 0
      } as AnswerResults,
      dummySelectedQuestionID: "141",
      answersIdStringCollection: '&answers[]=14030'
    }
  },
  methods: {},
  mounted() {
    axios
      .get(this.baseUrlAPI + 'submit&quizId=' + this.dummySelectedQuestionID + this.answersIdStringCollection)
      .then(({data}) => this.score = data)
  }
})
</script>

<style scoped>

</style>
