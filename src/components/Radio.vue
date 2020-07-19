<template>
  <div class="radio">
    <input
      role="radio"
      :id="id"
      type="radio"
      :name="group"
      tabindex="0"
      :value="value"
      :checked="checked"
      v-on:click="setPlateColor"
    />
    <label :for="id">{{name}}</label>
  </div>
</template>

<script>
export default {
  name: "Radio",
  props: {
    group: {
      Type: String,
      required: true
    },
    name: {
      Type: String,
      required: true
    },
    value: {
      Type: String,
      required: true
    },
    checked: Boolean
  },
  data() {
    return {
      id: this.name.toString().substring(0, 3)
    };
  },
  methods: {
    setPlateColor() {
      this.$emit("click", this.value);
    }
  }
};
</script>

<style scoped>
.radio {
  padding: 1em 0;
}

input[type="radio"] {
  display: block;
  opacity: 0;
  position: absolute;
  transition: color 0.5s cubic-bezier(0, 0.85, 1, 0.29);
  font-weight: bold;
}

input[type="radio"] + label {
  padding: 8px;
  cursor: pointer;
  border-radius: 10px;
  color: #8c8c8c;
  border-right: 0;
  font-weight: 600;
  letter-spacing: -0.7px;
  border: 2px solid transparent;
  transition: all 0.25s ease-in-out;
  box-shadow: 0 0 0 0 #00000045;
}

input[type="radio"]:hover + label {
  border: 2px solid #0038a8;
  color: #0038a8;
}

input[type="radio"]:checked + label {
  color: #fff;
  background-color: #0038a8;
  box-shadow: 9px 9px 8px 6px #00000045;
}

input[type="radio"] + label::after {
  content: "\2194";
  position: absolute;
  margin-top: -30px;
  margin-left: 0px;
  opacity: 0;
  letter-spacing: 1.2px;
  padding: 1px 6px;
  font-size: 1em;
  background-color: rgba(0, 0, 0, 0.5);
  border-radius: 8px;
}

input[type="radio"]:focus + label::after {
  animation: atribute 1s ease-in;
}

label {
  outline: 2px solid rgba(0, 0, 0, 0);
}

label:focus {
  outline: 2px solid #8c8c8c;
}

@keyframes atribute {
  0% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
  100% {
    opacity: 0;
  }
}
</style>