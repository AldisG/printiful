<template>
  <h2>Questions about: {...}</h2>
  <div class="question-container-grid">
    <div class="question-element">A</div>
    <div class="question-element">B</div>
    <div class="question-element">C</div>
    <div class="question-element">D</div>
  </div>
  <button class="button-next-and-submit">Next</button>
</template>

<script>
import axios from 'axios'
export default {
  name: "TheQuiz",
  props: {
    baseUrlAPI: String
  },
  mounted () {
    axios
    .get(this.baseUrlAPI + 'questions&quizId=141')
    .then((res) => console.log(res.data))
  }
//  if i know the category, do the api call
}
</script>

<style scoped>

</style>
