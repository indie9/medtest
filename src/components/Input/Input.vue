<template>
  <div class="input">
    <div class="input__wrapper">
      <input
      class="input__field"
        :type="typeInput"
        :placeholder="placeholder"
        :value="value"
        @input="updateValue($event.target.value)"
        :class="{
          'input__field--error': error,
        }"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Input",
  props: {
    typeInput: {
      default: "text",
      type: String,
    },
    value: String,
    placeholder: String,
    error: String,
  },
  methods: {
    updateValue(newValue) {
      this.$emit("input", newValue);
    },
  },
};
</script>

<style lang="sass" scoped>
  @import "index.sass"
</style>
