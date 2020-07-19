<template>
  <transition name="slide-fade" mode="in-out" appear>
    <div class="boardCard">{{content}}</div>
  </transition>
</template>

<script>
export default {
  name: "BoardCard",
  props: {
    content: String
  }
};
</script>

<style scoped>
.boardCard {
  padding: 12px 8px;
  border-radius: 6px;
  margin: 0 10px;
  background-color: #fff;
  font-weight: bold;
  letter-spacing: -0.8px;
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
  opacity: 0;
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>