<template>
  <div class="radio">
    <input
      :id="id"
      type="radio"
      :name="group"
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
    checked: {
      Type: Boolean,
      default: false
    }
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
  display: none;
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
</style>