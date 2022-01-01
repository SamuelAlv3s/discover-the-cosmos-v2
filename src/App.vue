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

:root {
  --black: #040911;
  --white: #fafafa;
  --red: #e83f5b;
  --background: #fbfcff;
  --blue: #3626a7;
}

body {
  background: var(--background);
}

.main {
  position: relative;
}
</style>
