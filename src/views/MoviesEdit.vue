<template>
  <div class="movies-edit container mt-5">
    <h1>Edit Movie</h1>

    <ul v-if="errors.length > 0" class="alert alert-danger">
      <li v-for="error in errors" v-bind:key="error">
        {{ error }}
      </li>
    </ul>
    <form v-on:submit.prevent="editMovie()">
      <div class="form-group">
        <label for="title">Title:</label>
        <input
          type="text"
          id="title"
          class="form-control"
          v-model="movie.title"
        />
      </div>
      <div class="form-group">
        <label for="year">Year:</label>
        <input
          type="text"
          id="year"
          class="form-control"
          v-model="movie.year"
        />
      </div>
      <div class="form-group">
        <label for="director">Director:</label>
        <input
          type="text"
          id="director"
          class="form-control"
          v-model="movie.director"
        />
      </div>
      <div class="form-group">
        <label for="image-url">Image URL:</label>
        <input
          type="text"
          id="image-url"
          class="form-control"
          v-model="movie.image_url"
        />
      </div>
      <div class="form-group">
        <label for="english">English?:</label>
        <input
          type="checkbox"
          id="english"
          v-model="movie.english"
          true-value="true"
          false-value="false"
        />
      </div>
      <div class="form-group">
        <label for="plot">Plot:</label>
        <input
          type="text"
          id="plot"
          class="form-control"
          v-model="movie.plot"
        />
      </div>
      <button class="btn btn-success mt-2">Update</button>
    </form>
  </div>
</template>

<style scoped>
label {
  display: inline-block;
  width: 80px;
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
      this.movie = response.data;
    });
  },
  methods: {
    editMovie: function() {
      var params = {
        title: this.movie.title,
        year: this.movie.year,
        director: this.movie.director,
        english: this.movie.english,
        plot: this.movie.plot,
        image_url: this.movie.image_url,
      };
      axios
        .patch(`/api/movies/${this.movie.id}`, params)
        .then(response => {
          console.log(response.data);
          this.$router.push(`/movies/${this.movie.id}`);
        })
        .catch(error => {
          console.log(error.response);
          if (error.response.status === 401) {
            this.errors = [
              "You must be logged in as an admin to edit a movie.",
            ];
          } else {
            console.log(error.response.data.errors);
            this.errors = error.response.data.errors;
          }
        });
    },
  },
};
</script>
