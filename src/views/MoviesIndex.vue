<template>
  <div class="movies-index container">
    <div class="top-buttons">
      <router-link to="/movies/new"
        ><button class="new-movie">New Movie</button></router-link
      >
    </div>
    <h1>Movies</h1>
    <div class="movie" v-for="movie in movies" v-bind:key="movie.title">
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

export default {
  data: function() {
    return {
      movies: [],
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
