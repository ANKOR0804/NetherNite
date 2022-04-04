<template>
  <vue-input v-model="searchQuery" placeholder="Enter package name" />

  <div class="row">
    <div class="col-sm-6">
      <button @click="fetchModules">Получить че-то</button>
    </div>
    <div class="col-sm-6">
      <vue-select v-model="selectedSort" :options="sortOptions" />
    </div>
  </div>
  <js-modules :jsModules="searchedModules" v-if="!isModulesLoading" />

  <div v-else>Идет загрузка...</div>

  <ul class="pagination">
    <li v-for="pageNumber in totalPages" :key="pageNumber" class="page-item">
      <a
        :class="{
          'lol': from === pageNumber
        }"
        class="page-link"
        href="#"
        >{{ pageNumber }}</a
      >
    </li>
  </ul>
</template>

<script>
import JsModules from "@/components/JsModules.vue";
import axios from "axios";
import VueSelect from "@/components/VueSelect.vue";
import VueInput from "@/components/VueInput.vue";

export default {
  components: {
    JsModules,
    VueSelect,
    VueInput,
  },

  data() {
    return {
      jsModules: [],
      showMore: false,
      isModulesLoading: false,
      selectedSort: "",
      searchQuery: "",
      from: 0,
      size: 10,
      totalPages: 10,
      sortOptions: [
        { value: "package.name", name: "Sort by name" },
        { value: "package.description", name: "Sort by description" },
      ],
    };
  },
  methods: {
    async fetchModules() {
      try {
        this.isModulesLoading = true;
        const response = await axios.get(
          "https://registry.npmjs.org/-/v1/search?",
          {
            params: {
              text: this.searchQuery,
              from: this.from,
              size: this.size,
            },
          }
        );
        console.log(response.data.objects);
        this.jsModules = response.data.objects;
      } catch (e) {
        alert("Somethin went wrong...");
      } finally {
        this.isModulesLoading = false;
      }
    },
  },
  mounted() {
    this.fetchModules();
  },
  computed: {
    sortedModules() {
      return [...this.jsModules].sort((jsModule1, jsModule2) => {
        return jsModule1[this.selectedSort]?.localeCompare(
          jsModule2[this.selectedSort]
        );
      });
    },

    searchedModules() {
      return this.sortedModules.filter((jsModule) =>
        jsModule.package.name
          .toLowerCase()
          .includes(this.searchQuery.toLowerCase())
      );
    },
  },
  watch: {},
};
</script>

<style lang="scss">
.lol {
  background: black;
}
</style>
