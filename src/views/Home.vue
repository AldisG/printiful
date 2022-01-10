<template>
  <div class="home" >
    <SelectQuiz v-if="!canTakeTest" @emmitUserChoiceUp="emitUserChoice" :baseUrlAPI="baseUrlAPI" />
    <TheQuiz v-if="canTakeTest" :baseUrlAPI="baseUrlAPI" :userChoice="userChoice" @reactivateQuizUpToHome="quizCompleteReset"/>
  </div>
  {{canTakeTest}}
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import {UserChoice} from '@/Types.ts'
import SelectQuiz from '@/components/SelectQuiz.vue'
import TheQuiz from '@/components/TheQuiz.vue'

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
  // @reactivateQuizUp="quizCompleteReset"
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
      // this.canTakeTest = false
    },
    quizCompleteReset() {
      // console.log('lllll')
      this.canTakeTest = false
    },
  }
})
</script>
