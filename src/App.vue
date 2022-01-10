<template>
  <router-view/>
</template>

<script>

export default ({})
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Spline+Sans:wght@300;500;700&display=swap');
$dark: #2c3e50;
$darkTransparent: #2c3e5050;
$white: #fff;

*, *::before, *::after{
  box-sizing: border-box;
  color: $dark;
  padding: 0;
  margin: 0;
}
input,
select,
button {
  width: 100%;
  padding: 10px;
  background-color: $white;
  outline: 1px solid $darkTransparent;
  border: none;
  border-radius: 4px;
  &:hover{
    outline: 1px solid $dark;
  }
  &:active, &:focus {
    outline: 2px solid $dark;
  }
}
button {
  cursor: pointer;
  font-size: 18px;
  user-select: none;
}
label{
  display: flex;
  flex-direction: column;
  align-items: self-start;
}
body{
  background-color: #2c3e50;
  font-weight: 300;
}

#app {
  font-family: 'Spline Sans', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding-top: 40px;
  display: flex;
  justify-content: center;
}
.home {
  background-color: $white;
  width: 500px;
  padding: 60px 40px;
}
</style>
