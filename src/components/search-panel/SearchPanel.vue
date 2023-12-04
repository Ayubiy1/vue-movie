<template>
  <input
    type="text"
    class="form-control search-input"
    placeholder="Kinoni qidirish"
    @input="searchMovieF"
  />
</template>

<script>
export default {
  methods: {
    searchMovieF(e) {
      this.$emit("searchMovie", e.target.value);
    },
  },
};
</script>

<style scoped>
.search-input {
  margin-bottom: 1rem;
  padding: 1rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
