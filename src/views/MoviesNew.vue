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
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title" />
      </div>
      <div class="form-group">
        <label>Year:</label>
        <input type="text" class="form-control" v-model="year" />
      </div>
      <div class="form-group">
        <label>Director:</label>
        <input type="text" class="form-control" v-model="director" />
      </div>
      <div class="form-group">
        <label>English?:</label>
        <input
          type="checkbox"
          v-model="english"
          true-value="true"
          false-value="false"
        />
      </div>
      <div class="form-group">
        <label>Plot:</label>
        <textarea
          cols="30"
          rows="10"
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
      english: "",
      plot: "",
      errors: [],
    };
  },
  created: function() {},
  methods: {
    createMovie: function() {
      var params = {
        title: this.title,
        year: this.year,
        plot: this.plot,
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
