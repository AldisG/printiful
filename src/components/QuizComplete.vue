<template>
  <div class="wrapper">
    <h2 class="quiz-complete">Quiz completed!</h2>
    <h4 class="user">Thanks "" NAME ""!</h4>
    <span class="user-final-score">{{ score }} out of {{ maxScore }}.</span>
  </div>
</template>

<script lang="ts">
export default ({
  name: "QuizComplete",
  props: {},
  data() {
    return {
      score: 0,
      maxScore: 10
    }
  },
  methods: {
    setUserScore() {
      console.log('Winer')
    }
  },
  mounted() {
    this.setUserScore()
  }
})
</script>

<style scoped>

</style>
