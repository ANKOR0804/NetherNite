<template>
  <select :content="modelValue" @change="changeOption">
    <option disabled value="">Chose from list</option>
    <option
      v-for="option in options"
      :key="option.value"
      :value="option.value"
    >
    {{option.name}}
    </option>
  </select>
</template>

<script>
export default {
  name: "vue-select",
  props: {
    modelValue: {
      type: String,
    },
    options: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
      changeOption(event) {
          this.$emit('update:modelValue', event.target.value)
      }
  }
};
</script>

<style>
</style>