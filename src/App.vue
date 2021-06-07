<template>
  
      <Movies msg="Lista de peliculas recomendadas" />
    
</template>

<script>
import Movies from "./components/Movies.vue";

export default {
  name: "App",
  components: {
    Movies,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
