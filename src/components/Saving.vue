<template>
  <div class="saving">
    <button class="button" aria-label="view plates" v-on:click="show = !show">
      <transition name="icon-fade" mode="out-in">
        <font-awesome-icon v-if="show == true" icon="eye" />
        <font-awesome-icon v-else-if="show == false" icon="eye-slash" />
      </transition>
    </button>
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
  margin-top: 20px;
  display: flex;
}

.button {
  padding: 8px 0px;
  min-width: 60px;
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

.icon-fade-enter-active {
  transition: all 0.3s ease;
}
.icon-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}
.sicon-fade-enter, .icon-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>