<template>
  <div class="about">
    <h1>A simple todo app with localStorage</h1>
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Created by Dennis Kjellin" />
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'AboutView',
  components: {
    HelloWorld
  }
}
</script>
