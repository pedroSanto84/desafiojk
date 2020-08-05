<template>
  <div class="container mb-3">
    <div class="d-flex mb-3">
      <div class="mr-auto">
        <h3>Resultado da pesquisa "{{ reformattedSearchString }}"</h3>
      </div>
      <div class="btn-group ml-auto" role="group">
        <button
          @click="changeDisplayMode('grid')"
          type="button"
          class="btn btn-outline-secondary"
          v-bind:class="{ active: displayMode === 'grid' }"
        >
          <i class="fas fa-th"></i>
        </button>
        <button
          @click="changeDisplayMode('list')"
          type="button"
          class="btn btn-outline-secondary"
          v-bind:class="{ active: displayMode === 'list' }"
        >
          <i class="fas fa-list"></i>
        </button>
      </div>
    </div>

    <div id="app" v-if="displayMode === 'grid'">
      <div class="card-columns" v-bind:key="video.id.videoId" v-for="video in videos">
        <VideoGrid v-bind:video="id" />
        <iframe
          id="app"
          type="text/html"
          width="640"
          height="360"
          :src="'//www.youtube.com/embed/' + video.id.videoId"
          frameborder="0"
          altplay="1"
        />
      </div>
    </div>
    <div v-else>
      <div class="card mb-2" v-bind:key="video.id.videoId" v-for="video in videos">
        <VideoList v-bind:video="video" />
      </div>
    </div>
  </div>
</template>

<script>
import VideoList from "./VideoList";
import VideoGrid from "./VideoGrid";

export default {
  name: "SearchResults",
  components: {
    VideoList,
    VideoGrid,
  },
  data() {
    return {
      title: "Search Results",
      displayMode: "grid",
    };
  },
  methods: {
    changeDisplayMode(displayMode) {
      this.displayMode = displayMode;
    },
  },
  props: ["videos", "reformattedSearchString"],
};
</script>

<style scoped>
button:focus {
  box-shadow: none !important;
}
</style>