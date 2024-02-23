<template>
  <div class="multi-select">
    <div class="multi-select__wrapper">
      <div @click="toggleDropdown" :class="{ 'multi-select__selected-option--error': isError }"
        class="multi-select__selected-option">
        <span v-if="!value.length">Выберите значение</span>
        <span v-else>{{ value.join(", ") }}</span>
        <svg class="multi-select__arrow" :class="{ 'multi-select__arrow--rotated': isOpen }"
          xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
          <path d="M7 10l5 5 5-5z" />
        </svg>
      </div>
      <ul v-show="isOpen" class="multi-select__options">
        <li v-for="option in options" :key="option" @click="toggleOption(option)"
          :class="{ 'multi-select__options--active': value.includes(option) }">
          {{ option }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "MultiSelect",
  data() {
    return {
      isOpen: false,
    };
  },
  props: {
    options: {
      type: Array,
      required: true,
    },
    value: {
      type: Array,
      default: [],
    },
    isError: Boolean,
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    toggleOption(option) {
      let res = [...this.value];
      const index = res.indexOf(option);

      if (index !== -1) {
        res.splice(index, 1);
      } else {
        res.push(option);
      }
      this.$emit("input", res);
    },
  },
};
</script>

<style lang="sass" scoped> 
  @import "index.sass"
</style>
