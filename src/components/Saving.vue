<template>
  <div class="saving">
    <button class="button" v-on:click="show = !show">Placas salvas</button>
    <transition name="slide">
      <div class="content" v-if="show">
        <BoardCard v-for="content in contents" :key="content" :content="content" />
      </div>
    </transition>
  </div>
</template>

<script>
import BoardCard from "./BoardCard";

export default {
  name: "Saving",
  components: {
    BoardCard
  },
  props: {
    contents: Array
  },
  data() {
    return {
      show: false
    };
  }
};
</script>

<style scoped>
.saving {
  position: relative;
  display: flex;
}

.button {
  padding: 12px 16px;
  background-color: #c8ceda;
  font-family: "Segoe UI Light";
  font-weight: bold;
  font-size: 1em;
  border: 0;
  color: rgb(131, 130, 130);
  cursor: pointer;
  outline: none;
  border: 2px solid rgba(0, 0, 0, 0);
  border-radius: 10px;
  transition: all 0.25s ease-in-out;
  box-shadow: 0 0 0 0 #00000045;
}

.button:hover {
  box-shadow: 9px 9px 8px 6px #00000045;
}

.button:disabled {
  background-color: #738ed8;
  cursor: not-allowed;
  color: #ddd;
}

.button:focus {
  border: 2px solid #fff;
}

.content {
  display: flex;
  padding: 0 16px;
}

.slide-enter-active,
.slide-leave-active {
  transition: all 0.5s;
}
.slide-enter, .slide-leave-to /* .fade-leave-active em versões anteriores a 2.1.8 */ {
  transform: translateY(30px);
  opacity: 0;
}
</style>