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
body {
  background: url("../src/assets/Layer.svg") no-repeat 0px 0px / contain;
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
  body {
    background: url("../src/assets/Layer-m.svg") no-repeat 0px 0px / contain;
  }
}
</style>
