<template>
  <div class="modal-wrapper">
    <div class="modal">
      <div class="close" @click="$emit('closeModal')">
        <img src="../assets/close-outline.svg" />
      </div>
      <div class="modal-img">
        <img :src="apod.hdurl" :alt="apod.title" />
      </div>
      <div class="modal-content">
        <div class="header">
          <b v-if="apod.copyright">by {{ apod.copyright }}</b>
          <small>{{ apod.date }}</small>
        </div>
        <h4>{{ apod.title }}</h4>
        <div class="explanation">
          <p>{{ apod.explanation }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "Modal",
  props: {
    apod: {},
  },
  emits: ["closeModal"],
};
</script>
<style scoped>
.modal-wrapper {
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 20px;
  left: 0;
  background: rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.modal {
  position: relative;
  width: 90%;
  height: 80%;
  background: var(--background);
  border-radius: 15px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  overflow: auto;
  animation: animate 0.5s ease-in-out;
}

.close {
  position: absolute;
  top: 10px;
  right: 10px;
  cursor: pointer;
}

.close img {
  width: 25px;
  height: 25px;
}

.modal-img {
  width: 50%;
  height: 100%;
  overflow: hidden;
  padding: 5px 0 5px 15px;
}

.modal-img img {
  width: 100%;
  height: 100%;
  object-fit: scale-down;
  image-rendering: optimizeQuality;
}

.modal-content {
  width: 50%;
  padding: 35px 15px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  row-gap: 5px;
  column-gap: 5px;
}

h4 {
  margin: 1px 0;
}

.explanation {
  padding: 5px;
  overflow: auto;
  border-radius: 5px;
  background: rgba(0, 0, 0, 0.02);
}

.modal::-webkit-scrollbar {
  width: 4px;
}

/* Track */
.modal::-webkit-scrollbar-track {
  background: var(--background);
}

/* Handle */
.modal::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.2);
}

/* Handle on hover */
.modal::-webkit-scrollbar-thumb:hover {
  background: rgba(0, 0, 0, 0.3);
}

.explanation p {
  text-align: justify;
}

@keyframes animate {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@media (max-width: 768px) {
  .modal {
    width: 90%;
    height: calc(100vh - 100px);
    flex-direction: column;
  }

  .modal-img {
    width: 100%;
    padding: 10px 5px;
  }

  .modal-img img {
    object-fit: cover;
  }

  .modal-content {
    width: 100%;
  }

  .close img {
    filter: invert(1);
  }
}
</style>
