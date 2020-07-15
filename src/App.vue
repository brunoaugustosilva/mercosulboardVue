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
}

body {
  background-color: #e6e6e6;
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

#app {
  display: flex;
  align-items: stretch;
  flex-flow: column;
  flex-wrap: wrap;
  padding: 1em;
  transition: all 0.5s ease-in-out;
  font-family: "Segoe UI Light";
  align-items: center;
}
</style>
