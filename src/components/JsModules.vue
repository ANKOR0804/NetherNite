<template>
  <div class="packages container">
    <h3>Список модулей</h3>
    <js-module
      v-for="jsModule in jsModules"
      :key="jsModule.id"
      :jsModule="jsModule"
    />
  </div>
</template>

<script>
import JsModule from "./JsModule.vue";
export default {
  components: { JsModule },
  props: {
    jsModules: {
      type: Array,
      required: true,
    },
  },
  methods: {
  }
};
</script>

<style lang="scss" scoped>
.packages {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-direction: column;
}
</style>