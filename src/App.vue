<template>
  <div id="app">
    <Main />
  </div>
</template>

<script>
import Main from "./components/Main.vue";

export default {
  name: "App",
  components: {
    Main
  }
};
</script>

<style>
*,
*:before,
*:after {
  box-sizing: border-box;
}

body,
html {
  margin: 0;
  padding: 0;
  min-height: 100%;
}

body {
  background-color: #e6e6e6;
  background-image: radial-gradient(
    50em at 97px 70%,
    #cbd0dc 30%,
    transparent 20%
  );
  background-size: 12em 100%;
  background-position: 0px 20em;
  background-repeat: repeat-x;
  color: #232121;
}

body::-webkit-scrollbar {
  width: 0.5em;
}

body::-webkit-scrollbar-track {
  box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
}

body::-webkit-scrollbar-thumb {
  outline: 1px solid #fff;
  background-color: #fff;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  color: #232121;
  letter-spacing: -1px;
}

mark {
  color: #232121;
  padding: 0 5px;
  background-color: #ccb82d;
  border-radius: 2px;
  box-shadow: 2px 3px 1px #00000045;
}

.text-center {
  text-align: center;
}

#app {
  display: flex;
  align-items: stretch;
  flex-flow: column;
  flex-wrap: wrap;
  padding: 1em;
  transition: all 0.5s ease-in-out;
  font-family: Inter, system-ui, -apple-system, "Roboto Regular", "Segoe UI";
  align-items: center;
}
</style>
