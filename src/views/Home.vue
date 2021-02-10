<template>
  <div class="home">
    <div id="new-movie" class="text-center">
      <h1>Add a New Movie</h1>
      <div>
        <label for="">Title: </label
        ><input type="text" v-model="newMovieTitle" />
      </div>
      <div>
        <label for="">Year: </label><input type="text" v-model="newMovieYear" />
      </div>
      <div>
        <label for="">Plot: </label><input type="text" v-model="newMoviePlot" />
      </div>
      <button v-on:click="createMovie">Create</button>
      <p class="errors">{{ errors.join(", ") }}</p>
    </div>
    <hr />
    <h1>All Movies</h1>
    <div id="movies">
      <div class="movie" v-for="movie in movies" v-bind:key="movie.title">
        <h3>{{ movie.title }}</h3>
        <p>Year: {{ movie.year }}</p>
        <p v-if="movie.genres != []">Genres: {{ movie.genres.join(", ") }}</p>
        <p>Plot: {{ movie.plot }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
h1,
h2 {
  text-align: center;
}
label {
  display: inline-block;
  width: 50px;
}
.home {
  text-align: left;
}
.movie {
  padding: 1em 0;
  margin: 0 1em;
  border-bottom: 1px solid grey;
}

.movie:last-child {
  border-bottom: 0;
}
.errors {
  color: red;
}
.text-center {
  text-align: center;
}
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Hello",
      movies: [],
      errors: [],
      newMovieTitle: null,
      newMovieYear: null,
      newMoviePlot: null,
    };
  },
  created: function() {
    axios.get("/api/movies").then((response) => {
      console.log(response.data);
      this.movies = response.data;
    });
  },
  methods: {
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
      };
      var options = {
        headers: {
          Authorization:
            "Bearer eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE2MTMwMDIyMzV9.OJ1_WJUoeVhMkqOkPeElxde0BuC_MNfC3LsLiyik7dg",
        },
      };
      axios
        .post("/api/movies", params, options)
        .then((response) => {
          // console.log(response.data);
          this.movies.unshift(response.data);
        })
        .catch((error) => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
