<template>
  <MenuStatic />
  <GeneralContainer />
</template>

<script>
import MenuStatic from "./components/Static/MenuStatic.vue"
import GeneralContainer from "./components/Static/GeneralContainer.vue"

export default {
  name: 'App',
  components: {
    MenuStatic,
    GeneralContainer,
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

//Fonts style for all web page
  font-style: normal;
  font-weight: 300;
  font-size: 12px;
  line-height: 120%;
  color: #FFF;

  display: flex;
}

body,html {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
