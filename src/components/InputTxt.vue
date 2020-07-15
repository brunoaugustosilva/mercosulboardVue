<template>
  <div class="input__container">
    <label for="text" v-bind:style="{color: color}">
      {{label}}
      <span v-show="required">*</span>
    </label>
    <input
      type="text"
      name="text"
      v-on:keyup="getInputValue"
      id="text"
      :placeholder="placeholder"
      v-model="value"
      v-on:keyup.enter="submitInput"
      :pattern="pattern"
      v-bind:style="{borderBottomColor: color}"
      :autofocus="focusable"
      :required="required"
      lang="pt-BR"
      :title="label"
      maxlength="7"
    />
    <span class="input_tip">{{tip}}</span>
  </div>
</template>

<script>
export default {
  name: "InputTxt",
  props: {
    label: String,
    placeholder: String,
    pattern: String,
    color: String,
    tip: String,
    length: Number,
    required: Boolean,
    focusable: Boolean
  },
  data() {
    return {
      value: ""
    };
  },
  methods: {
    getInputValue(input) {
      let inputValue = input.target.value;
      this.$emit("keyup", inputValue);
      return inputValue;
    },
    submitInput(input) {
      let inputValue = input.target.value;
      this.value = "";
      this.$emit("submitInput", inputValue);
    }
  }
};
</script>

<style scoped>
.input__container {
  padding: 4px;
  margin-top: 10px;
  display: flex;
  flex-flow: column nowrap;
}

input[type="text"] {
  font-family: "Segoe UI Light";
  font-size: 1.5em;
  padding: 6px 8px;
  margin: 2px 0 10px 0;
  border: 0;
  margin-top: 5px;
  text-transform: uppercase;
  background-color: transparent;
  border-bottom: 1px solid gray;
  outline: none;
}

input[type="text"]::placeholder {
  font-size: 1em;
}

input:-webkit-autofill {
  transition: all 5000s ease-in-out 0s;
  transition-property: background-color, color;
  /* if input has one color, and didn't have bg-image use shadow */
  -webkit-box-shadow: 0 0 0 1000px rgba(0, 0, 0, 0) inset;
  /* text color */
  -webkit-text-fill-color: rgb(0, 0, 0);
  /* font weigth */
  font-weight: 300 !important;
}

.input_tip {
  opacity: 0.6;
  font-size: 0.7em;
}
</style>