<template>
  <div id="app">
    <nav
      class="navbar navbar-expand-lg navbar-light"
      style="background-color:#f4c618;"
    >
      <router-link to="/" class="navbar-brand">VueMDB</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link to="/" class="nav-link">Movies</router-link>
          </li>
          <li v-if="!isLoggedIn()" class="nav-item">
            <router-link to="/signup" class="nav-link">Signup</router-link>
          </li>
          <li v-if="!isLoggedIn()" class="nav-item">
            <router-link to="/login" class="nav-link">Login</router-link>
          </li>
          <li v-if="isLoggedIn()" class="nav-item">
            <router-link to="/logout" class="nav-link">Logout</router-link>
          </li>
        </ul>
      </div>
    </nav>
    <router-view />
  </div>
</template>

<script>
export default {
  methods: {
    isLoggedIn: function() {
      return localStorage.getItem("jwt");
    },
  },
};
</script>

<style>
a {
  color: #3f51b5;
}

.top-buttons {
  text-align: right;
  margin-right: 1em;
}

.btn-success {
  background-color: #4caf50;
  border-color: #4caf50;
}
</style>
