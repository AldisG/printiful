<template>
  <div class="loading-message">
    <img class="loading-icon" src='../assets/loading.svg' alt="loading" />
    <span>{{ elementsText }}</span>
  </div>
</template>

<script lang="ts">
export default ({
  name: "LoadingComponent",
  props: {
    elementsText: String
  },
})
</script>

<style scoped>

.loading-message {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.loading-icon {
  box-sizing: content-box;
  width: 30px;
  padding: 10px;
  opacity: .3;
  animation-name: spin;
  animation-duration: 500ms;
  animation-iteration-count: infinite;
  animation-timing-function: linear;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

</style>
