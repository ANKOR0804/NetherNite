<template>
  <main class="main">

    <div class="input-group mb-3 pt-5">
      <vue-input
          v-model="searchQuery"
          type="text"
          class="form-control"
          placeholder="Enter a package name"
          aria-label="Enter a package name"
      />
      <button
          @click="fetchModules"
          class="btn btn-outline-primary"
          type="button"
          id="button-addon2"
      >
        Find package
      </button>
    </div>
    <vue-select v-model="selectedSort" :options="sortOptions" :style="{display:'none'}"/>

    <js-modules :jsModules="searchedModules" v-if="!isModulesLoading"/>

    <ul class="pagination" v-if="!isModulesLoading">
      <li v-for="pageNumber in totalPages"
          :key="pageNumber"
          :class="{
            'active': from === pageNumber,
          }"
          class="page-item">
        <a

            class="page-link"
            @click="changePage(pageNumber)"
        >{{ pageNumber }}</a
        >
      </li>
    </ul>

    <div v-else>Loading...</div>
  </main>

  <vue-footer/>
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
      from: 1,
      size: 10,
      totalPages: 5,
      sortOptions: [
        {value: "package.name", name: "Sort by name"},
        {value: "package.description", name: "Sort by description"},
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
        this.jsModules = response.data.objects;
        this.isModulesLoading = true;
      } catch (e) {
        alert("Something went wrong...");
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
};
</script>

<style lang="scss">
.page-wrapper {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.page-item {
  cursor: pointer;
}
</style>
