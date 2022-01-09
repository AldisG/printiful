<template>
  <div class="home">
    <SelectQuiz v-if="!canTakeTest" :baseUrlAPI="baseUrlAPI" @emmitUserChoiceUp="emitUserChoice" />
    <TheQuiz v-if="canTakeTest" :baseUrlAPI="baseUrlAPI" :userChoice="userChoice"/>
  </div>
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
    }
  }
})
</script>
