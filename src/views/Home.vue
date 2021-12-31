<template>
  <div class="card-container">
    <figure id="card" v-for="(apod, index) in apodImages" :key="index">
      <img :src="apod.url" :alt="apod.title" loading="lazy" />
    </figure>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      apodImages: null,
    };
  },
  methods: {
    async getRandomImages() {
      const request = await fetch(
        `https://api.nasa.gov/planetary/apod?api_key=0yDrBxsNT9gQZDsQVx0i26KWg7xHyYfANQakmgFj&count=20`
      );

      const response = await request.json();

      console.log(response);

      this.apodImages = response;
    },
  },
  mounted() {
    this.getRandomImages();
  },
};
</script>
<style scoped>
.card-container {
  margin-top: 60px;
  padding: 20px 30px;
  width: 100%;
  height: 100%;
  column-count: 4;
  column-gap: 10px;
}

figure {
  margin: 0;
  display: flex;
  break-inside: avoid;
  margin-bottom: 15px;
}

figure img {
  width: 100%;
  border-radius: 15px;
  transition: transform 0.3s ease-in-out;
}

figure:hover img {
  transform: scale(1.01);
}
</style>
