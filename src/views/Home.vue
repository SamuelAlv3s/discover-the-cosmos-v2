<template>
  <div class="row" v-if="gallery.length">
    <div class="column" v-for="(item, index) in gallery" :key="index">
      <figure v-for="(item2, index) in item" :key="index">
        <img :src="item2.url" :alt="item2.title" @load="loadImage" />
      </figure>
    </div>
  </div>
  <div v-if="showMainLoading" class="loading-container">
    <h1 data-text="Loading...">Loading...</h1>
  </div>
  <Modal v-if="showModal" :apod="apod" @close-modal="closeModal" />
  <div v-show="showSentinel" id="sentinel"></div>
  <div class="loading" v-show="showLoading">
    <img src="../assets/loading.gif" alt="loading" />
  </div>
  <div class="error-wrapper" v-show="showError">
    <div class="error">
      <img src="../assets/error500.gif" alt="error 500" />
    </div>
  </div>
</template>

<script>
import Modal from "../components/Modal.vue";
export default {
  name: "Home",
  data() {
    return {
      showModal: false,
      apiKey: process.env.VUE_APP_API_KEY,
      isLoaded: false,
      showSentinel: false,
      gallery: [],
      showMainLoading: false,
      showLoading: false,
      showError: false,
    };
  },
  components: {
    Modal,
  },
  methods: {
    async getRandomImages() {
      this.showMainLoading = true;
      try {
        const request = await fetch(
          `https://api.nasa.gov/planetary/apod?api_key=${this.apiKey}&count=20`
        );

        const response = await request.json();

        const filteredData = response.filter(
          (item) => item.media_type !== "video" && item.url
        );

        for (let i = 0; i < 4; i++) {
          this.gallery.push(
            filteredData.slice(
              i * (filteredData.length / 4),
              (i + 1) * (filteredData.length / 4)
            )
          );
        }
      } catch (error) {
        console.log(error);
        this.showError = true;
      } finally {
        this.showMainLoading = false;
      }
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
          try {
            this.showLoading = true;
            const request = await fetch(
              `https://api.nasa.gov/planetary/apod?api_key=${this.apiKey}&count=20`
            );

            const response = await request.json();

            const filteredData = response.filter(
              (item) => item.media_type !== "video" && item.url
            );

            this.gallery.forEach((el, i) =>
              el.push(...filteredData.slice(i * 5, (i + 1) * 5))
            );
          } catch (error) {
            console.log(error);
          } finally {
            this.showLoading = false;
          }
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
  font-size: clamp(1em, 4em, 8em);
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
  margin-top: 60px;
  display: flex;
  flex-wrap: wrap;
  padding: 15px 30px;
}

.column {
  flex: 25%;
  max-width: 25%;
  padding: 0 4px;
}

@media (max-width: 1024px) {
  .column {
    flex: 33.33%;
    max-width: 33.33%;
  }
}

@media (max-width: 768px) {
  .column {
    flex: 50%;
    max-width: 50%;
  }
}

@media (max-width: 480px) {
  .column {
    flex: 100%;
    max-width: 100%;
  }

  .error {
    width: 250px;
    height: 250px;
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
  position: absolute;
  bottom: 250px;
  width: 100%;
  height: 25px;
  background: transparent;
}

.loading {
  width: 50px;
  height: 50px;
  position: fixed;
  bottom: 10%;
  left: 50%;
  background: var(--blue);
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  animation: toUp 1s forwards ease-in-out;
}

.loading img {
  width: 50px;
  height: 50px;
}

.error-wrapper {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.error {
  width: 500px;
  height: 500px;
  border-radius: 50%;
  overflow: hidden;
}

.error img {
  width: 100%;
  height: 100%;
  object-fit: cover;
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

@keyframes toUp {
  0% {
    width: 0;
    height: 0;
    opacity: 0;
    bottom: 0;
  }
  100% {
    width: 50px;
    height: 50px;
    opacity: 1;
    bottom: 10%;
  }
}
</style>
