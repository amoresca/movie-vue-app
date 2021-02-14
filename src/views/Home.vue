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
      <div class="errors" v-for="error in errors" :key="error">
        <p>{{ error }}</p>
      </div>
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
var jwt = localStorage.getItem("jwt");
if (jwt) {
  axios.defaults.headers.common["Authorization"] = "Bearer " + jwt;
}

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
    axios.get("/api/movies").then(response => {
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
      axios
        .post("/api/movies", params)
        .then(response => {
          // console.log(response.data);
          this.movies.unshift(response.data);
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
