<template>
  <input class="button" type="button" :value="value" v-on:click="submit" :disabled="disabled" />
</template>

<script>
export default {
  name: "Button",
  props: {
    value: String,
    disabled: Boolean
  },
  methods: {
    submit() {
      this.$emit("click");
    }
  }
};
</script>

<style scoped>
.button {
  padding: 12px 16px;
  background-color: #0038a8;
  font-family: "Segoe UI Light";
  font-weight: bold;
  font-size: 1em;
  border: 0;
  color: #fff;
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
</style>