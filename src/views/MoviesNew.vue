<template>
  <div class="movies-index container">
    <h1>New Movie</h1>

    <ul>
      <li class="text-danger" v-for="error in errors" v-bind:key="error">
        {{ error }}
      </li>
    </ul>
    <form v-on:submit.prevent="createMovie()">
      <div class="form-group">
        <label for="title">Title:</label>
        <input type="text" id="title" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label for="year">Year:</label>
        <input type="text" id="year" class="form-control" v-model="year" />
      </div>
      <div class="form-group">
        <label for="director">Director:</label>
        <input
          type="text"
          id="director"
          class="form-control"
          v-model="director"
        />
      </div>
      <div class="form-group">
        <label for="genre">Genre:</label>
        <select id="genre" class="form-control" v-model="genre">
          <option v-for="genre in genres" :key="genre.id" :value="genre.id">{{
            genre.name
          }}</option>
        </select>
      </div>
      <div class="form-group">
        <label for="english">English?:</label>
        <input
          type="checkbox"
          id="english"
          v-model="english"
          true-value="true"
          false-value="false"
        />
      </div>
      <div class="form-group">
        <label for="plot">Plot:</label>
        <textarea
          cols="30"
          rows="10"
          id="plot"
          class="form-control"
          v-model="plot"
          maxlength="750"
        ></textarea>
        <small v-bind:class="{ 'text-danger': plot.length === 750 }">
          {{ plot.length }}/750 characters</small
        >
      </div>
      <button>Create</button>
    </form>
  </div>
</template>

<style scoped>
label {
  display: inline-block;
  width: 80px;
}
.text-danger {
  color: red;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      title: "",
      year: "",
      director: "",
      genres: [],
      genre: "",
      english: "",
      plot: "",
      errors: [],
    };
  },
  created: function() {
    axios.get("/api/genres").then(response => {
      this.genres = response.data;
    });
  },
  methods: {
    createMovie: function() {
      var params = {
        title: this.title,
        year: this.year,
        plot: this.plot,
        genre_id: this.genre,
      };
      axios
        .post("/api/movies", params)
        .then(response => {
          var newMovie = response.data;
          console.log(newMovie);
          this.$router.push(`/movies/${newMovie.id}`);
        })
        .catch(error => {
          console.log(error.response);
          if (error.response.status === 401) {
            this.errors = [
              "You must be logged in as an admin to create a movie.",
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
