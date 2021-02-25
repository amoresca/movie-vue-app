<template>
  <div class="movies-index container">
    <div class="top-buttons">
      <router-link v-if="this.$parent.isLoggedIn()" to="/movies/new"
        ><button class="new-movie">New Movie</button></router-link
      >
    </div>
    <div class="text-center">
      <h1>Movies</h1>
      <label for="search">Search by title:</label>
      <input v-model="titleFilter" id="search" list="titles" />
      <datalist id="titles">
        <option v-for="movie in movies" v-bind:key="movie.id">{{
          movie.title
        }}</option>
      </datalist>
      | <label for="filter">Sort By: </label>
      <select id="filter" v-model="orderAttribute">
        <option value="title">Title</option>
        <option value="year">Year</option>
      </select>
    </div>
    <div
      class="movie"
      v-for="movie in orderBy(
        filterBy(movies, titleFilter, 'title'),
        orderAttribute
      )"
      v-bind:key="movie.title"
    >
      <router-link :to="`/movies/${movie.id}`"
        ><h3>{{ movie.title }}</h3></router-link
      >
      <p>Year: {{ movie.year }}</p>
      <p v-if="movie.genres.length > 0">
        Genres: {{ movie.genres.join(", ") }}
      </p>
      <p>Plot: {{ movie.plot }}</p>
    </div>
  </div>
</template>

<style scoped>
.movie {
  padding: 1em 0;
  margin: 0 1em;
  border-bottom: 1px solid grey;
}

.movie:last-child {
  border-bottom: 0;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      titleFilter: "",
      orderAttribute: "title",
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      // console.log(response.data);
      this.movies = response.data;
    });
  },
  methods: {},
};
</script>
