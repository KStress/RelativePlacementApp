<template>
  <div id="app">
    <RPCalc />
  </div>
</template>

<script>
import RPCalc from "@/components/rp-calc";

export default {
  name: "App",
  components: {
    RPCalc,
  },
};
</script>

<style>
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.pointer {
  cursor: pointer;
}
</style>
