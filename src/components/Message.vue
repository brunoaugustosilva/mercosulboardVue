<template>
  <div class="message" v-bind:style="{backgroundColor: color}">
    <div class="message_title">{{title}}</div>
    <div class="message_text">{{text}}</div>
  </div>
</template>

<script>
export default {
  name: "Message",
  props: {
    title: String,
    text: String,
    color: String
  }
};
</script>

<style scoped>
.message {
  padding: 12px 16px;
  position: absolute;
  border-radius: 8px;
  font-family: "Segoe UI Light";
  color: #fff;
  left: 40px;
  top: 20px;
  animation: message 2s forwards cubic-bezier(0.55, 0.06, 0.68, 0.19);
}

.message_title {
  font-size: 0.7em;
  text-transform: uppercase;
  padding: 2px 0;
}

.message_text {
  text-transform: lowercase;
  font-size: 1em;
}
</style>