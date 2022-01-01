<template>
  <div class="card-container" v-if="apodImages">
    <figure id="card" v-for="(apod, index) in apodImages" :key="index">
      <img
        :src="apod.url"
        :alt="apod.title"
        loading="lazy"
        @click="openModal(apod)"
      />
    </figure>
  </div>
  <div v-else class="loading-container">
    <h1 data-text="Loading...">Loading...</h1>
  </div>
  <Modal v-if="showModal" :apod="apod" @close-modal="closeModal" />
</template>

<script>
import Modal from "../components/Modal.vue";
export default {
  name: "Home",
  data() {
    return {
      apodImages: null,
      apod: null,
      showModal: false,
    };
  },
  components: {
    Modal,
  },
  methods: {
    async getRandomImages() {
      const request = await fetch(
        `https://api.nasa.gov/planetary/apod?api_key=0yDrBxsNT9gQZDsQVx0i26KWg7xHyYfANQakmgFj&count=20`
      );

      const response = await request.json();

      this.apodImages = response.filter((item) => item.media_type !== "video");
    },
    openModal(apodData) {
      this.apod = apodData;
      this.showModal = true;
      window.scrollTo({ top: 0, behavior: "smooth" });
      document.querySelector("body").style.overflow = "hidden";
    },
    closeModal() {
      this.showModal = false;
      document.querySelector("body").style.overflow = "initial";
    },
  },
  mounted() {
    this.getRandomImages();
  },
};
</script>
<style scoped>
.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #fafafa;
}

.loading-container h1 {
  position: relative;
  font-size: 8em;
  color: #222;
  text-transform: uppercase;
  border-bottom: 16px solid #222;
  letter-spacing: 0.05em;
  line-height: 1.2em;
}

.loading-container h1:before {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  color: #03f4e8;
  overflow: hidden;
  border-bottom: 16px solid #03f4e8;
  animation: animate 2s linear infinite;
}

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
  max-width: 100%;
  overflow: hidden;
  border-radius: 15px;
  animation: lazy 2.5s ease-in-out;
}

figure img {
  width: 100%;
  transition: transform 0.3s ease-in-out;
  image-rendering: optimizeSpeed;
  animation: lazy 2.5s ease-in-out;
}

figure:hover img {
  transform: scale(1.1);
}

@keyframes lazy {
  0% {
    opacity: 0;
    width: 80%;
    height: 80%;
  }
  100% {
    opacity: 1;
    width: 100%;
    height: 100%;
  }
}

@keyframes animate {
  0% {
    width: 0;
  }
  100% {
    width: 100%;
  }
}
</style>
