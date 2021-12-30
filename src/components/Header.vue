<template>
  <header>
    <img src="../assets/planet-outline.svg" alt="Logo" />
    <div id="nav">
      <router-link to="/" active-class="active">Home</router-link>
      <router-link to="/about" active-class="active">About</router-link>
    </div>
    <router-view />
  </header>
</template>
<script>
export default {
  name: "Header",
};
</script>
<style scoped>
header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background: #fafafa;
  padding: 10px 100px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: flex-start;
  column-gap: 5px;
}

header img {
  width: 35px;
  margin-right: 15px;
}

header a {
  text-decoration: none;
  padding: 8px 20px;
  border-radius: 20px;
  font-weight: bold;
  color: #0a0a0a;
}

header a.active {
  background: #0a0a0a;
  color: #fafafa;
}
</style>
