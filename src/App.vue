<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <RPCalc />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import RPCalc from "@/components/RPCalc";

export default {
  name: "App",
  components: {
    //HelloWorld,
    RPCalc,
  },
};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-top: 60px;
}
</style>
