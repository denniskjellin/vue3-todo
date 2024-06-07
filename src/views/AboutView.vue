<template>
  <div class="about">
    <h1>A Simple Todo App with localStorage</h1>
    <p>This is a simple todo application built using Vue.js. It allows you to manage your tasks effectively.</p>
    <h2>Features:</h2>
    <ul>
      <li>Add new tasks</li>
      <li>Delete tasks</li>
      <li>Toggle task completion</li>
      <li>Sort tasks by date (ascending or descending)</li>
      <li>Tasks are stored in localStorage, so they persist even if you close the browser</li>
    </ul>
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Created by Dennis Kjellin" />
  </div>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'AboutView',
  components: {
    HelloWorld
  }
}
</script>

<style scoped>
.about {
  text-align: center;
  margin-top: 20px;
}


ul {
  list-style-type: none;
  padding: 0;
  margin-top: 10px;
}

ul li {
  margin-bottom: 5px;
}

img {
  margin-top: 20px;
  max-width: 200px;
}
</style>
