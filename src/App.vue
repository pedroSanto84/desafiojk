<template>
  <div id="app">
    <Header />
    <ListForm v-on:search="search" />
    <ListResults
      v-if="videos.length > 0"
      v-bind:videos="videos"
      v-bind:reformattedSearchString="reformattedSearchString"
    />
  </div>
</template>

<script>
import Header from "./components/layout/Header";
import ListForm from "./components/ListForm";
import ListResults from "./components/ListResults";
import axios from "axios";

export default {
  name: "app",

  components: {
    Header,
    ListForm,
    ListResults,
  },

  data() {
    return {
      videos: [],
      reformattedSearchString: "",
      api: {
        baseUrl: "https://www.googleapis.com/youtube/v3/search?",

        part: "snippet",
        type: "video",
        order: "viewCount",
        maxResults: 10,

        q: "",
        key: "AIzaSyCo5mboWbjqi2TItaXW6v2wH7rWYQJDFtk",
        prevPageToken: "",
        nextPageToken: "",
      },
    };
  },

  methods: {
    search(searchParams) {
      this.reformattedSearchString = searchParams.join(" ");
      this.api.q = searchParams.join("+");
      const { baseUrl, part, type, order, maxResults, q, key } = this.api;
      const apiUrl = `${baseUrl}part=${part}&type=${type}&order=${order}&q=${q}&maxResults=${maxResults}&key=${key}`;
      this.getData(apiUrl);
    },

    prevPage() {
      const {
        baseUrl,
        part,
        type,
        order,
        maxResults,
        q,
        key,
        prevPageToken,
      } = this.api;
      const apiUrl = `${baseUrl}part=${part}&type=${type}&order=${order}&q=${q}&maxResults=${maxResults}&key=${key}&pageToken=${prevPageToken}`;
      this.getData(apiUrl);
    },

    nextPage() {
      const {
        baseUrl,
        part,
        type,
        order,
        maxResults,
        q,
        key,
        nextPageToken,
      } = this.api;
      const apiUrl = `${baseUrl}part=${part}&type=${type}&order=${order}&q=${q}&maxResults=${maxResults}&key=${key}&pageToken=${nextPageToken}`;
      this.getData(apiUrl);
    },

    getData(apiUrl) {
      axios.get(apiUrl).then((res) => {
        this.videos = res.data.items;

        this.api.prevPageToken = res.data.prevPageToken;
        this.api.nextPageToken = res.data.nextPageToken;
      }).catch;
    },
  },
};
</script>
