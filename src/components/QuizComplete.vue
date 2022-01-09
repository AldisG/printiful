<template>
  <div class="wrapper">
    <h2 class="quiz-complete">Quiz completed!</h2>
    <h4 class="user">Thanks, {{userNameUpperCase}}!</h4>
    <span v-if="score" class="user-final-score">
      Your score is {{ score.correct }} / {{ score.total }}!
    </span>
  </div>
</template>

<script lang="ts">
import axios from "axios";
import {AnswerResults} from "@/Types";
import {UserChoice} from '@/Types.ts'

export default ({
  name: "QuizComplete",
  props: {
    baseUrlAPI: String,
    userAnswerListIntoString: String,
    userName: String,
    userChoice: {} as UserChoice,
  },
  data() {
    return {
      score: {correct: 0, total: 0} as AnswerResults,
      selectedQuestionID: this.userChoice.selectedID,
      userNameUpperCase: this.userChoice.selectedUserName[0].toUpperCase() + this.userChoice.selectedUserName.slice(1)
    }
  },
  methods: {},
  async mounted() {
    await axios
      .get(this.baseUrlAPI + 'submit&quizId=' + this.selectedQuestionID + this.userAnswerListIntoString)
      .then(({data}) => this.score = data)
  },
})
</script>

<style scoped>

</style>
