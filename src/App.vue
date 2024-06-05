<template>
  <div id="app">
    <nav>
      <router-link to="/">Tasks</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {

  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;

}

body {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  background: linear-gradient(to right, #37443f, #00495d);
  height: 100vh;
  display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #d8d8d8;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
