<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/movies">Movies</router-link> |
      <router-link to="/signup">Signup</router-link> |
      <router-link to="/login">Login</router-link> |
      <router-link to="/logout">Logout</router-link>
    </div>
    <router-view />
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

#nav {
  padding: 30px;
  text-align: center;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

h1,
h2 {
  text-align: center;
}

a {
  color: #3f51b5;
}

label {
  display: inline-block;
}

.container {
  max-width: 1024px;
  margin: 0 auto;
  position: relative;
}

.top-buttons {
  text-align: right;
  margin-right: 1em;
}

.top-buttons button {
  color: white;
  border: 0;
  border-radius: 5px;
  padding: 5px 10px;
  font-weight: bold;
  cursor: pointer;
}

.new-movie,
.edit-movie {
  background-color: #42b983;
}

.delete-movie {
  background-color: #bf261b;
  margin-left: 5px;
}

.errors {
  color: red;
}

.text-center {
  text-align: center;
}
</style>
