<template>
  <ul class="movi-list list-group">
    <MoviListItem
      v-for="movie in movies"
      :movie="movie"
      :key="movie.id"
      @onRevome="$emit('onRevome', movie.id)"
      @onToggle="$emit('onToggle', $event)"
    />
  </ul>
</template>

<script>
import MoviListItem from "./MoviListItem.vue";

export default {
  components: { MoviListItem },
  props: {
    movies: {
      type: Array,
      required: true,
    },
    filterValue: {
      type: String,
      required: true,
    },
  },
  methods: {},
};
</script>

<style scoped>
.movi-list {
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
}
</style>
