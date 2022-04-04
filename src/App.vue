<template>
  <div class="container">
    <div class="input-group mb-3 pt-5">
      <vue-input
        v-model="searchQuery"
        type="text"
        class="form-control"
        placeholder="Enter package name"
        aria-label="Enter a package name"
      />
      <button
        @click="fetchModules"
        class="btn btn-outline-secondary"
        type="button"
        id="button-addon2"
      >
        Find package
      </button>
    </div>
    <vue-select v-model="selectedSort" :options="sortOptions" :style="{display:'none'}" />

    <js-modules :jsModules="searchedModules" v-if="!isModulesLoading" />

    <ul class="pagination" v-if="!isModulesLoading">
      <li v-for="pageNumber in totalPages" :key="pageNumber" class="page-item">
        <a
          :class="{
            lol: from === pageNumber,
          }"
          class="page-link"
          @click="changePage(pageNumber)"
          >{{ pageNumber }}</a
        >
      </li>
    </ul>

    <div v-else>Идет загрузка...</div>

    <vue-footer />
  </div>
</template>

<script>
import JsModules from "@/components/JsModules.vue";
import axios from "axios";
import VueSelect from "@/components/VueSelect.vue";
import VueInput from "@/components/VueInput.vue";
import VueFooter from "@/components/VueFooter.vue"

export default {
  components: {
    JsModules,
    VueSelect,
    VueInput,
    VueFooter
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
      totalPages: 5,
      sortOptions: [
        { value: "package.name", name: "Sort by name" },
        { value: "package.description", name: "Sort by description" },
      ],
    };
  },
  methods: {
    changePage(pageNumber) {
      this.from = pageNumber;
      this.fetchModules();
    },

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
  watch: {
    page() {
      this.fetchModules();
    },
  },
};
</script>

<style lang="scss">
  .page-item {
    cursor: pointer;
  }
</style>
