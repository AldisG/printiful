<template>
  <div class="quiz-complete-container">
    <h2 class="quiz-complete">Quiz completed!</h2>
    <h4 class="user-congrats">Thanks, {{ userNameUpperCase }}!</h4>

    <LoadingComponent v-if="!score.total" elementsClass="user-final-score" elementsText="Calculating results!" />

    <span v-if="score.total" class="user-final-score">
      Your score is <b>{{ score.correct }} / {{ score.total }}</b>!
    </span>
  </div>
  <button @click="$emit('reactivateQuizUp', 'emit me')" class="restart-button">New Quiz</button>
</template>

<script lang="ts">
import axios from "axios"
import {AnswerResults} from "@/Types"
import {UserChoice} from '@/Types.ts'
import LoadingComponent from '@/components/LoadingComponent.vue'

export default ({
  name: "QuizComplete",
  components: {
    LoadingComponent
  },
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
      .catch((err) => console.log('Error fetching answers', err))
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
