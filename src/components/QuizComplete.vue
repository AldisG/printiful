<template>
  <div class="quiz-complete-container">
    <h2 class="quiz-complete">Quiz completed!</h2>
    <h4 class="user-congrats">Thanks, {{ userNameUpperCase }}!</h4>
    <span v-if="!score.total" class="user-final-score">
      Thinking!
    </span>
    <span v-if="score.total" class="user-final-score">
      Your score is <b>{{ score.correct }} / {{ score.total }}</b>!
    </span>
  </div>
  <button @click="$emit('reactivateQuizUp', 'emit me')" class="restart-button">New Quiz</button>
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
  emits: ["reactivateQuizUp"],

  data() {
    return {
      score: {correct: 0, total: 0} as AnswerResults,
      selectedQuestionID: this.userChoice.selectedID,
      userNameUpperCase: this.userChoice.selectedUserName[0].toUpperCase() + this.userChoice.selectedUserName.slice(1)
    }
  },
  async mounted() {
    await axios
      .get(this.baseUrlAPI + 'submit&quizId=' + this.selectedQuestionID + this.userAnswerListIntoString)
      .then(({data}) => this.score = data)
  },
})
</script>

<style scoped>
.quiz-complete-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.quiz-complete {
  font-size: 44px;
  padding-bottom: 10px;
}

.user-congrats,
.user-final-score {
  font-size: 24px;
}

.restart-button {
  margin-top: 40px;
}
</style>
