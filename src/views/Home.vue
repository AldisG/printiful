<template>
  <div class="home">
    <SelectQuiz v-if="!canTakeTest" @emmitUserChoiceUp="emitUserChoice" :baseUrlAPI="baseUrlAPI" />
    <TheQuiz v-if="canTakeTest" :baseUrlAPI="baseUrlAPI" :userChoice="userChoice"
             @reactivateQuizUpToHome="quizCompleteReset" />
  </div>

  {{ canTakeTest }}
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import {UserChoice} from '@/Types.ts'
import SelectQuiz from '@/components/SelectQuiz.vue'
import TheQuiz from '@/components/TheQuiz.vue'
import axios from 'axios'

export default defineComponent({
  name: 'Home',
  data() {
    return {
      baseUrlAPI: "https://printful.com/test-quiz.php?action=",
      canTakeTest: false,
      userChoice: {} as UserChoice
    }
  },
  emits: ["emmitUserChoiceUp2"],
  components: {
    SelectQuiz,
    TheQuiz,
  },
  methods: {
    allowTakeTest() {
      this.canTakeTest = true
    },
    emitUserChoice(userData: UserChoice) {
      this.userChoice = userData
      this.allowTakeTest()
    },
    quizCompleteReset() {
      this.canTakeTest = false
    },
  }
})
</script>
<style lang="scss">
$white: #fff;

.home {
  min-height: 350px;
  height: max-content;
  box-sizing: border-box;
  background-color: $white;
  width: 700px;
  padding: 60px 40px;
  border-radius: 14px;
}
</style>
