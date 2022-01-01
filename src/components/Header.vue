<template>
  <header>
    <img src="../assets/planet-outline.svg" alt="Logo" />
    <div id="nav">
      <router-link to="/" active-class="active">Galeria</router-link>
      <router-link to="/picture-day" active-class="active">
        Imagem do Dia
      </router-link>
    </div>
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
  width: 100%;
  height: 60px;
  top: 0;
  left: 0;
  width: 100%;
  background: var(--background);
  padding: 10px 100px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: flex-start;
  column-gap: 5px;
  z-index: 10;
}

@media (max-width: 768px) {
  header {
    padding: 5px 20px;
    justify-content: space-between;
  }

  #nav {
    width: 100%;
    display: flex;
    justify-content: flex-end;
  }
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
  color: var(--black);
}

header a.active {
  background: var(--black);
  color: var(--white);
}
</style>
