<template>
  <div class="home">
    This is home
    <SelectQuiz :baseUrlAPI="baseUrlAPI" />
    <TheQuiz v-if="canTakeTest" :baseUrlAPI="baseUrlAPI" />
    <QuizComplete v-if="canTakeTest" />
  </div>
  <button @click="allowTakeTest()">Allow? {{ canTakeTest }}</button>
</template>

<script lang="ts">
import {defineComponent} from 'vue'
import SelectQuiz from '@/components/SelectQuiz.vue'
import TheQuiz from '@/components/TheQuiz.vue'
import QuizComplete from "@/components/QuizComplete.vue";

export default defineComponent({
  name: 'Home',
  data() {
    return {
      baseUrlAPI: "https://printful.com/test-quiz.php?action=",
      canTakeTest: false
    }
  },
  components: {
    SelectQuiz,
    TheQuiz,
    QuizComplete
  },
  methods: {
    allowTakeTest() {
      this.canTakeTest = true
    }
  }
})
</script>
