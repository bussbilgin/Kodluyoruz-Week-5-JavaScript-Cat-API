<template>
  <div class="home">
    <h2 class="page__header">CAT</h2>
    <div class="images" v-if="images.length > 0">
      <div
        v-for="image in this.images" :key="image.id" class="images__container">
        <img :src="image.url" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      images: [],
    };
  },
  created() {
    this.getImages();
  },

  methods: {
    async getImages() {
      try {
        let query_params = {
          breed_ids: "acur",
          limit: 8,
        };

        let response = await axios.get(
          "https://api.thecatapi.com/v1/images/search",
          { params: query_params }
        );

        this.pagination_count = response.headers["pagination-count"];
        this.images = response.data;
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style lang="scss">
.home {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  justify-content: flex-start;

  &__header {
    flex-grow: 1;

    margin: 0;
    padding: 20px;

    min-height: 120px;

    display: flex;
    flex-direction: row;
    align-items: flex-start;
    justify-content: flex-start;

    background-color: $blue;
    color: $white;
  }

  .images {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;

    &__container {
      width: 300px;
      height: 300px;
    }

    &__container img {
      width: 300px;
      height: auto;
      max-height: 100%;
    }
  }
}
</style>
