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
  padding: 40px 100px;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.card {
  width: 100%;
  display: flex;
  align-items: center;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.25);
  border-radius: 15px;
}

.card .image {
  width: 40%;
  height: 100%;
  padding: 10px;
}

.image img {
  width: 100%;
  height: max-content;
  border-radius: 15px 0 0 15px;
}

.card .content {
  width: 60%;
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
</style>
