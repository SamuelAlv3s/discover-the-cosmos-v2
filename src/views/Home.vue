<template>
  <button v-if="apodImages" @click="getRandomImages">More Images</button>
  <!-- <div class="card-container" v-if="apodImages">
    <figure id="card" v-for="(apod, index) in apodImages" :key="index">
      <img
        v-show="isLoaded"
        :src="apod.url"
        :alt="apod.title"
        @load="loadImage"
        @click="openModal(apod)"
      />
    </figure>
  </div> -->
  <div class="row" v-if="gallery.length">
    <div class="column" v-for="(item, index) in gallery" :key="index">
      <figure v-for="(item2, index) in item" :key="index">
        <img :src="item2.url" :alt="item2.title" :load="loadImage">
      </figure>
    </div>
  </div>
  <div v-else class="loading-container">
    <h1 data-text="Loading...">Loading...</h1>
  </div>
  <Modal v-if="showModal" :apod="apod" @close-modal="closeModal" />
  <div v-show="showSentinel" id="sentinel"></div>
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
      apiKey: process.env.VUE_APP_API_KEY,
      isLoaded: false,
      showSentinel: false,
      gallery: []
    };
  },
  components: {
    Modal,
  },
  methods: {
    async getRandomImages() {
      this.apodImages = null;
      const request = await fetch(
        `https://api.nasa.gov/planetary/apod?api_key=${this.apiKey}&count=20`
      );

      const response = await request.json();

      this.apodImages = response.filter((item) => item.media_type !== "video");
      console.log(this.apodImages);

      for( let i = 0; i < 4; i++) {
        this.gallery.push(this.apodImages.slice(i*(this.apodImages.length/4), (i+1)*(this.apodImages.length / 4)))
      }

      console.log(this.gallery);
    },
    openModal(apodData) {
      this.apod = apodData;
      this.showModal = true;
      document.querySelector("body").style.overflow = "hidden";
    },
    closeModal() {
      this.showModal = false;
      document.querySelector("body").style.overflow = "initial";
    },
    loadMore() {
      const intersectionObserver = new IntersectionObserver(async (entries) => {
        if (entries.some((entry) => entry.isIntersecting)) {

          const request = await fetch(
            `https://api.nasa.gov/planetary/apod?api_key=${this.apiKey}&count=20`
          );

          const response = await request.json();

          this.apodImages.push(
            ...response.filter((item) => item.media_type !== "video")
          );

          console.log("adicionou: ", this.apodImages);


           this.gallery.forEach((el, i) => el.push(...response.slice(i*5, (i+1)*5)))
        }
      });

      intersectionObserver.observe(document.querySelector("#sentinel"));

      return () => intersectionObserver.disconnect();
    },
    loadImage() {
      this.isLoaded = true;
      this.showSentinel = true;
    },
  },
  mounted() {
    this.getRandomImages();
    this.loadMore();
  },
};
</script>
<style scoped>
.loading-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  min-height: 100vh;
  background: var(--background);
}

.loading-container h1 {
  position: relative;
  font-size: clamp(1em, 6em, 8em);
  color: var(--black);
  text-transform: uppercase;
  border-bottom: 16px solid var(--black);
  letter-spacing: 0.05em;
  line-height: 1.2em;
}

.loading-container h1:before {
  content: attr(data-text);
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  color: var(--blue);
  overflow: hidden;
  border-bottom: 16px solid var(--blue);
  animation: animate 2s linear infinite;
}

button {
  margin-top: 70px;
  margin-left: 30px;
  padding: 10px;
  outline: none;
  border: none;
  border-radius: 5px;
  background: var(--black);
  color: var(--white);
  cursor: pointer;
  transition: filter 0.5s;
}

button:hover {
  filter: brightness(1.1);
}

.row {
  display: flex;
  flex-wrap: wrap;
  padding: 15px 30px;
}

.column {
  flex: 25%;
  max-width: 25%;
  padding: 0 4px;
}

.card-container {
  padding: 20px 30px;
  width: 100%;
  height: 100%;
  column-count: 4;
  column-gap: 10px;
}

@media (max-width: 1024px) {
  .card-container {
    column-count: 3;
  }
}

@media (max-width: 768px) {
  .card-container {
    column-count: 2;
  }
}

@media (max-width: 480px) {
  .card-container {
    column-count: 1;
  }
}

figure {
  margin: 0;
  display: flex;
  break-inside: avoid;
  margin-bottom: 15px;
  max-width: 100%;
  overflow: hidden;
  border-radius: 15px;
  animation: lazy 3.5s ease-in-out;
}

figure img {
  width: 100%;
  min-height: 100px;
  transition: transform 0.3s ease-in-out;
  image-rendering: optimizeSpeed;
  animation: lazy 3.5s ease-in-out;
}

figure:hover img {
  transform: scale(1.1);
}

#sentinel {
  width: 100%;
  height: 25px;
  background: transparent;
}

@keyframes lazy {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
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
