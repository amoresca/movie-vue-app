<template>
  <div class="movies-index">
    <div class="jumbotron jumbotron-fluid text-center bg-dark text-light">
      <div class="container">
        <div class="top-buttons">
          <router-link v-if="this.$parent.isLoggedIn()" to="/movies/new"
            ><button class="btn btn-success">
              New Movie
            </button></router-link
          >
        </div>
        <h1 class="display-4">Movies</h1>
        <form class="form-inline justify-content-center">
          <label class="mb-2 mr-2" for="search">Search by title:</label>
          <input
            type="text"
            class="form-control mb-2 mr-sm-2"
            v-model="titleFilter"
            id="search"
            list="titles"
          />

          <label class="mb-2 mr-2" for="filter">Sort By:</label>
          <select
            class="custom-select mb-2 mr-sm-2"
            id="filter"
            v-model="orderAttribute"
          >
            <option value="title" selected>Title</option>
            <option value="year">Year</option>
          </select>
        </form>
      </div>
    </div>

    <!-- Movies -->
    <div class="container">
      <div class="row row-cols-1 row-cols-md-2 row-cols-lg-3">
        <div
          class="col mb-4"
          v-for="movie in orderBy(
            filterBy(movies, titleFilter, 'title'),
            orderAttribute
          )"
          v-bind:key="movie.title"
        >
          <div class="card text-dark">
            <router-link
              :to="`/movies/${movie.id}`"
              class="image-link stretched-link"
              ><img :src="movie.image_url" class="card-img-top" alt="..."
            /></router-link>
            <div class="card-body">
              <h5 class="card-title">{{ movie.title }}</h5>
              <h6 class="card-subtitle mb-2 text-muted">{{ movie.year }}</h6>
              <p class="card-text">
                {{ movie.plot.substring(0, 90) + "..." }}
              </p>
            </div>
            <div class="card-footer bg-white text-muted">
              <router-link :to="`/movies/${movie.id}`">Read more</router-link>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
@media (min-width: 737px) {
  .image-link {
    height: 150px;
    overflow: hidden;
    position: relative;
  }
  .image-link img {
    position: absolute;
  }
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
