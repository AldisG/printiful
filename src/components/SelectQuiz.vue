<template>
  <div class="form-container">
    <h1 class="main-header">Quiz select</h1>
    <form class="form" @submit.prevent>
      <div class="form-elements">
        <label for="select-username">
          Username
          <input
            id="select-username"
            class="username"
            v-model="userName"
            type="text"
            maxlength="20">
        </label>
        <div>
          <label for="selected">Select a topic
            <select class="select-category" id="selected" v-model="quizTopic">
              <option
                v-for="category in apiQuizCategories"
                :key="category.id"
                :value="category.title">
                {{ category.title }}
              </option>
            </select>
          </label>
        </div>
      </div>
      <span class="form-error" v-if="errorMessage">{{ errorMessage }}</span>
      <button class="submit-button" @click="this.userSelectedNameAndTopic">Start</button>
    </form>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import {QuestionsAndAnswers} from '@/Types'

export default ({
  name: 'SelectQuiz',
  props: {
    baseUrlAPI: String,
  },
  emits: ["reactivateQuizUp", "reactivateQuizUpToHome"],

  mounted() {
    axios.get(this.baseUrlAPI + 'quizzes')
      .then(({data}) => this.apiQuizCategories = data)
      .catch((error) => console.log('Error catching server data', error));
  },
  data() {
    return {
      userName: '',
      quizTopic: '',
      apiQuizCategories: [] as QuestionsAndAnswers[],
      errorMessage: '',
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
      const chosenQuizTopic = this.apiQuizCategories.find((question) => {
        return question.title === this.quizTopic
      }).id
      this.$emit('emmitUserChoiceUp', {
        selectedUserName: this.userName,
        selectedID: chosenQuizTopic
      })
    },
  },
})
</script>

<style scoped lang="scss">
$red: #d32f2f;

.form-container,
.form-elements,
.form {
  display: flex;
  flex-direction: column;
  width: 100%;
  align-items: center;
}

.form-container, .form {
  gap: 40px;
}

.main-header {
  font-size: 44px;
}

.form-elements {
  width: max-content;
  gap: 20px;
  position: relative;

  & .username,
  & .select-category {
    padding: 10px;
    width: 300px;
  }
}

.form-error {
  text-align: left;
  font-size: 12px;
  position: absolute;
  top: 345px;
  left: calc(50% - 150px);
  color: $red;
  font-weight: 500;
}

.username,
.select-category {
  font-size: 18px;
}

.submit-button {
  width: 300px;
}
</style>
