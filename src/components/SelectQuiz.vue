<template>
  <div class="form-container">
    <h1>Quiz select</h1>
    <form class="form-elements" @submit.prevent="this.userSelectedNameAndTopic">
      <input v-model="userName" type="text">
      <select id="topic" v-model="quizTopic" name="topic">
        <option v-for="category in apiQuizCategories" :key="category.id" :value="category.title">
          {{ category.title }}
        </option>
      </select>
      <span v-if="errorMessage">{{ errorMessage }}</span>
      <button>Confirm</button>
    </form>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import { QuestionsAndAnswers } from '@/Types'

export default ({
  name: 'SelectQuiz',
  props: {
    baseUrlAPI: String,
  },
  data() {
    return {
      userName: '',
      quizTopic: '',
      apiQuizCategories: [] as QuestionsAndAnswers[],
      errorMessage: '',
      selectedQuestionID: 0
    }
  },
  methods: {
    userSelectedNameAndTopic(): void {
      if (this.userName.length < 3) {
        this.errorMessage = 'Username has to be at least 3 characters long'
        return
      } else if (!this.quizTopic) {
        this.errorMessage = 'Category must be selected'
        return
      } else {
        this.errorMessage = ''
      }
      this.selectedQuestionID = this.apiQuizCategories.find((question) => {
        return question.title === this.quizTopic
      }).id
      // console.log('Emmit this up' selectedQuestionID)
    }
  },
  mounted() {
    axios.get(this.baseUrlAPI + 'quizzes')
      .then(({data}) => this.apiQuizCategories = data)
      .catch((error) => console.log('Error catching server data', error));
  },
})
</script>

<style scoped>
.form-container,
.form-elements {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
}

.form-elements {
  width: max-content;
  align-items: stretch;
}
</style>
