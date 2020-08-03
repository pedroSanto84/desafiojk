<template>
  <div id="app">
    <Header />
    <SearchForm v-on:search="search" />
    <ListResults
      v-if="videos.length > 0"
      v-bind:videos="videos"
      v-bind:reformattedSearchString="reformattedSearchString"
    />
    <Pagination
      v-if="videos.length > 0"
      v-bind:prevPageToken="api.prevPageToken"
      v-bind:nextPageToken="api.nextPageToken"
      v-on:prev-page="prevPage"
      v-on:next-page="nextPage"
    />
  </div>
</template>

<script>
export default {
  name: "Pagination",
  props: ["prevPageToken", "nextPageToken"],
  methods: {
    prevPage() {
      this.$emit("prev-page");
    },
    nextPage() {
      this.$emit("next-page");
    },
  },
};
</script>

<style scoped>
.page-link {
  box-shadow: none !important;
}
</style>
