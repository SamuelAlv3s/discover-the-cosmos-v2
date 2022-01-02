<template>
  <div class="container" v-if="apod">
    <div class="card">
      <div class="image">
        <img :src="apod.url" :alt="apod.title" />
      </div>
      <div class="content">
        <div class="card-header">
          <div>
            <b>by {{ apod.copyright }}</b>
            <small>{{ apod.date }}</small>
          </div>
          <h2>{{ apod.title }}</h2>
        </div>
        <div class="card-body">
          <p>{{ apod.explanation }}</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "PictureDay",
  data() {
    return {
      apod: null,
    };
  },
  methods: {
    async getPictureOfTheDay() {
      const request = await fetch(
        `https://api.nasa.gov/planetary/apod?api_key=0yDrBxsNT9gQZDsQVx0i26KWg7xHyYfANQakmgFj`
      );

      const response = await request.json();

      console.log(response);

      this.apod = response;
    },
  },
  mounted() {
    this.getPictureOfTheDay();
  },
};
</script>
<style scoped>
.container {
  margin-top: 60px;
  padding: 20px 100px;
  width: 100%;
  height: calc(100vh - 60px);
  display: block;
}

.card {
  width: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.25);
  border-radius: 15px;
}

.card .image {
  width: 100%;
  padding: 10px;
}

.image img {
  width: 100%;
  height: max-content;
}

.card .content {
  width: 100%;
  padding: 10px;
}

.card-header {
  display: flex;
  flex-direction: column;
}

.card-header > div {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 0;
}

.card-header h2 {
  margin-bottom: 15px;
}

.card-body {
  padding: 15px;
  background: rgba(0, 0, 0, 0.05);
}
.card-body p {
  text-align: justify;
}

@media (max-width: 768px) {
  .container {
    padding: 10px;
  }
}
</style>
