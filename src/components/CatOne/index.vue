<template>
  <div class="search-page">
    <div class="hits-page">
      <Filters :showFilter="showFilter" />
      <div class="hits-wrapper">
        <div class="sort-and-stat">
          <ais-stats />
          <ais-configure :userToken="userToken()" />
          <ais-sort-by
            :items="[
              {
                value: 'rayban_merged',
                label: 'Relevancy',
              },
              {
                value: 'rayban_merged_price_dsc',
                label: 'Price Desc',
              },
              {
                value: 'rayban_merged_price_asc',
                label: 'Price Asc.',
              },
            ]"
          />
        </div>
        <transition name="fade">
          <ais-hits>
            <div
              @click="selectedProduct(item), svgClick()"
              class="hits-wrapper"
              slot="item"
              slot-scope="{ item }"
            >
              <div class="image-wrapper">
                <img :src="item.image_link" alt="" />
              </div>
              <div class="infos">
                <ais-highlight attribute="title" :hit="item" />
              </div>
            </div>
          </ais-hits>
        </transition>
        <ais-pagination />
      </div>
    </div>
  </div>
</template>

<script>
import Filters from "@/components/Filters";
import { mapGetters, mapActions } from "vuex";
export default {
  name: "catOne",
  data() {
    return {
      showFilter: true,
    };
  },
  components: {
    Filters,
  },
  methods: {
    showFiltersMethod() {
      this.showFilter = !this.showFilter;
    },
    userToken() {
      if (this.getPersonnaSelected == "Ben") {
        return "RB_Ben";
      }
      if (this.getPersonnaSelected == "Tiffany") {
        return "RB_Tiffany";
      }
      if (this.getPersonnaSelected == "Neutral") {
        return "Neutral";
      }
    },
    ...mapActions("SearchModule", ["selectedProduct"]),
    ...mapActions("HeaderModule", ["svgClick"]),
  },
  computed: {
    ...mapGetters("PersonnaModule", ["getPersonnaSelected"]),
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/scss/variables/variables.scss";
@import "@/assets/scss/mixin.scss";

.search-page {
  width: 90%;
  margin: 0 auto;
  .hits-page {
    display: flex;
    width: 90%;
    margin: 0 auto;
    .hits-wrapper {
      .sort-and-stat {
        padding: 0 5em;
      }
    }
  }
  .search {
    width: 80%;
    margin: 0 auto;
  }
  .filterBtn {
    width: 10%;
    margin: 0 0 0 5.4em;
    display: -webkit-flex;
    display: flex;
    cursor: pointer;
    position: relative;
    text-transform: uppercase;
    justify-content: space-between;
  }
  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s ease;
  }

  .fade-enter-from,
  .fade-leave-to {
    opacity: 0;
  }
}
</style>
