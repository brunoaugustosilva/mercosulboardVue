<template>
  <div id="singleLine">
    <div class="singleLineHeader">
      <font-awesome-icon :icon="icon" :title="header" />
      <span v-show="label == true">{{header}}</span>
    </div>
    <div class="singleLineContent">{{content}}</div>
  </div>
</template>

<script>
export default {
  name: "SingleLine",
  props: {
    header: String,
    content: String,
    icon: String,
    label: Boolean
  }
};
</script>

<style scoped>
#singleLine {
  display: flex;
  flex-flow: row;
  opacity: 0.8;
  padding: 1em;
  justify-content: space-between;
  border-bottom: 1px solid #80808052;
}

.singleLineHeader {
  font-weight: 700;
  letter-spacing: -1.2px;
}
</style>