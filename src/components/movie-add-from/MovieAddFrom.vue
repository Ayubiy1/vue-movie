<template>
  <div class="movie-add-form" @submit.prevent>
    <h3>Yangi kino qo'shish</h3>
    <form class="d-flex">
      <input
        type="text"
        class="form-control new-movie-label"
        placeholder="Qanday kino"
        :value="name"
        @input="name = $event.target.value"
      />
      <input
        type="number"
        class="form-control new-movie-label"
        placeholder="Necha marotaba ko'rilgan"
        :value="viewers"
        @input="viewers = $event.target.value"
      />
      <button class="btn btn-outlin-dark" @click="createMovie">Qo'shish</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return { name: "", viewers: "" };
  },

  props: {
    movies: {
      type: Array,
      required: true,
    },
  },

  methods: {
    createMovie() {
      const newMovie = {
        name: this.name,
        viewrs: this.viewers,
        favourite: false,
        like: false,
        id: this.movies?.length + 1,
      };

      if (this.name || this.viewrs) {
        console.log(this.name);
        this.$emit("createMovie", newMovie);
        this.name = "";
        this.viewers = "";
      }
    },
  },
};
</script>

<style scoped>
.movie-add-form {
  margin-top: 2rem;
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
