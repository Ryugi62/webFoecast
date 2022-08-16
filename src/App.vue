<template>
  <HelloWorld />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  
  components: {
    HelloWorld
  },
}
</script>

<style>
* {
  border: 1px solid red;
}

html,
body {
  margin: 0;
  height: 100%;
}
</style>
