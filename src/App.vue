<template>
  <GogleMapCustom :google="google"></GogleMapCustom>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import GogleMapCustom from './components/Gmap.vue'

export default {
  name: 'App',
  components: {
    GogleMapCustom
  },
  data() {
        return {
            google: window.google
        }
    },
  greated() {
  },
  mounted() {
    //console.log(google)
  }
}
</script>

<style>
#app {
  display: block;
  min-height: 100vh;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
