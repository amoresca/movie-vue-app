<template>
  <div class="movies-index container mt-5">
    <!-- Wait for genres to be loaded -->
    <div v-if="movie.genres">
      <div class="top-buttons" v-if="this.$parent.isLoggedIn()">
        <router-link :to="`/movies/${movie.id}/edit`"
          ><button class="btn btn-success">Edit Movie</button></router-link
        >
        <button class="btn btn-danger ml-3" v-on:click="destroyMovie()">
          Delete Movie
        </button>
      </div>
      <ul v-if="errors.length > 0" class="alert alert-danger">
        <li v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>

      <h1>{{ movie.title }}</h1>
      <img :src="movie.image_url" :alt="movie.title" />
      <p>Year: {{ movie.year }}</p>
      <p v-if="movie.genres.length > 0">
        Genres: {{ movie.genres.join(", ") }}
      </p>
      <p v-if="movie.director">Director: {{ movie.director }}</p>
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
      movie: {},
      errors: [],
    };
  },
  created: function() {
    axios.get(`/api/movies/${this.$route.params.id}`).then(response => {
      // console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    destroyMovie: function() {
      if (confirm("Are you sure you want to delete this movie?")) {
        axios
          .delete(`/api/movies/${this.movie.id}`)
          .then(response => {
            console.log(response.data);
            this.$router.push("/movies");
          })
          .catch(error => {
            console.log(error.response);
            if (error.response.status === 401) {
              this.errors = [
                "You must be logged in as an admin to delete a movie.",
              ];
            } else {
              console.log(error.response.data.errors);
              this.errors = error.response.data.errors;
            }
          });
      }
    },
  },
};
</script>
