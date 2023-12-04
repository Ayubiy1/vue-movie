<template>
  <li
    class="list-group-item d-flex align-items-center justify-content-between"
    :class="[{ like: movie.like }, { favourite: movie.favourite }]"
    @click="$emit('onToggle', { id: movie.id, prop: 'like' })"
    style="cursor: pointer"
  >
    <span class="list-group-item-label">{{ movie.name }}</span>

    <div class="d-flex justify-content-center align-items-center btn-group">
      <input type="text" class="list-group-item-input" :value="movie.viewrs" />

      <span
        class="btn-cookie btn-sm btn"
        @click="$emit('onToggle', { id: movie.id, prop: 'favourite' })"
      >
        <i class="fas fa-cookie"></i>
      </span>
      <span class="btn-trash btn-sm btn" @click="onRevome">
        <i class="fas fa-trash"></i>
      </span>

      <i class="fas fa-star"></i>
    </div>
  </li>
</template>
np
<script>
export default {
  props: {
    movie: {
      type: Object,
      required: true,
    },
  },
  methods: {
    onLike() {
      this.$emit("onLike", this.movie.id);
    },
    onRevome() {
      this.$emit("onRevome", this.movie.id);
    },
  },
};
</script>

<style scoped>
.list-group-item {
  padding: 15px;
  border-bottom: 2px solid #3d5a80 !important;
  border: none;
}

.list-group-item:last-child {
  border-bottom: none;
}
.list-group-item input {
  line-height: 35px;
  font-size: 20px;
  text-align: center;
  border: 0;
  outline: none;
}
.list-group-item button {
  width: 30px;
  height: 30px;
  margin: 3px;
  font-size: 15px;
  cursor: pointer;
  border: none;
  border-radius: 3px;
  background-color: rgba(128, 128, 128, 0.235);
}
.list-group-item .fa-star {
  width: 35px;
  height: 35px;
  text-align: center;
  line-height: 35px;
  font-size: 16px;
  color: #ffd700;
  transition: 0.3s all;
  transform: translate(30px);
  opacity: 0;
}
.list-group-item.like .fa-star {
  transform: translate(0);
  opacity: 1;
}
.list-group-item.favourite .list-group-item-label,
.list-group-item.favourite .list-group-item-input,
.btn-cookie {
  color: #e09f3e;
}
.btn-trash {
  color: red;
}
</style>
