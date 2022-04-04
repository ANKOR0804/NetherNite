<template>
  <input
    :value="modelValue"
    @input="updateInput"
    type="text"
    class="form-control"
    aria-describedby="inputGroup-sizing-lg"
  />
</template>

<script>
export default {
  name: "vue-input",
  props: {
    modelValue: [String, Number],
  },
  methods: {
    updateInput(event) {
      this.$emit("update:modelValue", event.target.value);
    },
  },
};
</script>

<style>
</style>