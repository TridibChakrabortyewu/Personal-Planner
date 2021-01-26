<template>
  <Navbar />
  <router-view />
</template>


<script>
import Navbar from "./components/Navbar";

export default {
  components: { Navbar },
};
</script>

<style>
body {
  background: #f2f2f2;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 600px;
  margin: 0 auto;
  color: #555;
}
@media (max-width: 425px) and (min-width: 320px) {
}
</style>
