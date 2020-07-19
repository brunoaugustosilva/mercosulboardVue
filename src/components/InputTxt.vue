<template>
  <div class="input__container">
    <label for="text">
      {{label}}
      <span v-show="required">*</span>
    </label>
    <input
      type="text"
      name="text"
      v-on:keyup="getValue"
      v-model="value"
      id="text"
      :placeholder="placeholder"
      :pattern="pattern"
      v-bind:style="{borderBottomColor: color}"
      :autofocus="focusable"
      :required="required"
      autocapitalize="on"
      spellcheck="false"
      autocomplete="false"
      lang="pt-BR"
      :title="label"
      maxlength="7"
    />
    <div class="tip__column">
      <transition name="slide-fade">
        <span class="input_tip" :key="tip" v-bind:style="{color: color}">{{tip}}</span>
      </transition>
    </div>
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
    getValue(input) {
      let inputValue = input.target.value;
      this.$emit("keyup", inputValue);
      this.value = input.target.value;
    },
    clean() {
      this.value = "";
    }
  },
  created() {
    this.$parent.$on("clean", this.clean);
  }
};
</script>

<style scoped>
label {
  font-weight: 500;
  letter-spacing: -1.2px;
  margin-bottom: 4px;
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
  transition: color 0.8s ease-in;
  outline: none;
}

input[type="text"]::placeholder {
  font-family: "Segoe UI Light";
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

.tip__column {
  display: flex;
}

.input_tip {
  font-family: "Segoe UI Light";
  font-size: 0.7em;
  position: absolute;
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter,
.slide-fade-leave-to
/* .slide-fade-leave-active for <2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
</style>