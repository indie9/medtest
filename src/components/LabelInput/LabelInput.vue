<template>
  <div class="label">
    <label class="label__message">
      <span v-if="necessarily" class="label__asterisk">*</span>{{ label }}
    </label>
    <div class="label__wrapper">
      <slot></slot>
      <label class="label__error" v-if="error">{{ error }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "Label",
  props: {
    label: String,
    necessarily: Boolean,
    error: String,
  },
};
</script>

<style lang="sass" scoped>
@import "index.sass"
</style>
