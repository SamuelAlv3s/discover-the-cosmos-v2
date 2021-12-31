<template>
  <div class="main">
    <Header />
    <router-view />
  </div>
</template>
<script>
import Header from "./components/Header.vue";
export default {
  name: "App",
  components: {
    Header,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Helvetica, sans-serif;
}

body {
  background: #fafafa;
}

.main {
  position: relative;
}
</style>
