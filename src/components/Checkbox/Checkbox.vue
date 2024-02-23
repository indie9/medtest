<template>
  <div class="checkbox">
    <input
      class="checkbox__input"
      type="checkbox"
      :id="id"
      :checked="value"
      @change="toggleCheckbox"
    />
    <label class="checkbox__label" :for="id">{{ label }}</label>
  </div>
</template>

<script>
export default {
  name: "CustomCheckbox",
  props: {
    id: {
      type: String,
      required: true,
    },
    label: {
      type: String,
      required: true,
    },
    value: Boolean,
  },
  data() {
    return {};
  },
  methods: {
    toggleCheckbox() {
      this.$emit("input", !this.value);
    },
  },
};
</script>

<style lang="sass" scoped>
  @import "index.sass"
</style>
