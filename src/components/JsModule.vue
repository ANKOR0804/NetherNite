<template>
  <div class="package" @click="moreInfo">
    <div class="row">
      <div class="col-sm-8">
        <a
          class="package__name"
          :href="`https://www.jsdelivr.com/package/npm/${jsModule.package.name}`"
          target="_blank"
          >{{ jsModule.package.name }}</a
        >
      </div>
      <div class="col-sm-4">
        <div class="package-buttons">
          <a
            class="button"
            :href="link"
            target="_blank"
            v-for="link in jsModule.package.links"
            :key="link.id"
          >
            <i class="bi bi-box-arrow-up-right"></i>
          </a>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12">
        <a
          class="package-owner me-3 mb-3"
          :href="`mail-to:${jsModule.package.publisher.email}`"
        >
          <i class="bi bi-patch-check"></i>
          {{ jsModule.package.publisher.username }}
        </a>
        <span class="package-label">
          <i class="bi bi-tag-fill"></i>
          {{ jsModule.package.version }}
        </span>
        <p class="package-description">{{ jsModule.package.description }}</p>
      </div>
    </div>
  </div>
  <click-modal v-model:show="showMore">
    <h3 class="mb-3">{{jsModule.package.name}}</h3>
    <p class="package-search-score">
      Search rating: {{ jsModule.searchScore }}
    </p>
    <p class="package-score-final">Final score: {{ jsModule.score.final }}</p>
    <div class="package-keywords">
      <span class="package-label me-2"
       v-for="keyword in jsModule.package.keywords"
       :key="keyword.id">
       <i class="bi bi-tag-fill mr-2"></i>{{keyword}}</span>
    </div>
  </click-modal>
</template>

<script>
import ClickModal from "@/components/ClickModal.vue";

export default {
  components: {
    ClickModal,
  },
  data() {
    return {
      showMore: false,
    };
  },
  props: {
    jsModule: {
      type: Object,
      required: true,
    },
  },
  methods: {
    moreInfo() {
      this.showMore = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.package {
  width: 100%;
  cursor: pointer;
  padding: 15px;
  border-radius: 10px;
  margin-bottom: 20px;

  &:hover {
    background-color: #e5e5e5;
  }

  &__name {
    text-align: left;
    font-size: 24px;
    font-weight: 500;
    color: #000;
    margin: 0;
  }

  &-buttons {
    display: flex;
    justify-content: flex-end;

    .button {
      display: inline-block;
      width: 40px;
      height: 40px;
      border: 1px solid #e5e5e5;
      font-size: 20px;
      line-height: 20px;
      text-align: center;
      padding: 10px;
      color: #000;
      opacity: 0.6;
      transition: opacity 0.2s;

      &:hover {
        background-color: #e5e5e5;
        color: blue;
      }
    }
  }
}
</style>