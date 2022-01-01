<template>
  <div class="modal-wrapper">
    <div class="modal">
      <div class="close" @click="$emit('closeModal')">
        <img src="../assets/close-outline.svg" />
      </div>
      <div class="modal-img">
        <img :src="apod.url" :alt="apod.title" />
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
  position: absolute;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.modal {
  position: relative;
  width: 800px;
  height: 450px;
  background: #fafafa;
  border-radius: 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  overflow: hidden;
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
}

.modal-img img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.modal-content {
  width: 50%;
  padding: 10px 15px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  row-gap: 5px;
}

h4 {
  margin: 10px 0;
}

.explanation {
  height: 310px;
  padding: 5px;
  max-height: 310px;
  overflow: auto;
  border-radius: 5px;
  background: rgba(0, 0, 0, 0.08);
}

.explanation::-webkit-scrollbar {
  width: 4px;
}

/* Track */
.explanation::-webkit-scrollbar-track {
  background: #fafafa;
}

/* Handle */
.explanation::-webkit-scrollbar-thumb {
  background: rgba(0, 0, 0, 0.2);
}

/* Handle on hover */
.explanation::-webkit-scrollbar-thumb:hover {
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
</style>
