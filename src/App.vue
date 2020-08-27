<template>
  <div id="app">
    <div class="container">
      <h1>What would you like to convert?</h1>
      <Converter />
    </div>
  </div>
</template>

<script>
import Converter from "./components/Converter";
export default {
  name: "App",
  components: {
    Converter,
  },
};
</script>

<style>
html,
body,
#app {
  min-height: 100vh;
  min-width: 100vw;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.container {
  height: 100vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #4bb543;
}

h1 {
  font-size: 40pt;
  color: #fff;
}
</style>
