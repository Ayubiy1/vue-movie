<template>
  <div class="app-info">
    <p class="fs-3 text-uppercase">Barcha kinolar soni: {{ allCountMovies }}</p>
    <p class="fs-4 text-uppercase">
      Ko'rilgan kinolar soni: {{ favouriteMoviesCount }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    allCountMovies: {
      typeof: Number,
      required: true,
    },
    favouriteMoviesCount: {
      typeof: Number,
      required: true,
    },
  },
};
</script>

<style scoped>
.app-info {
  padding: 1.5em;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
