<template>
  <div class="btn-group mt-10">
    <button
      v-for="button in buttons"
      class="btn btn-outline-dark"
      :key="button.name"
      :class="[activeFilter == button.name ? 'btn-dark' : 'btn-outline-dark']"
      :style="[activeFilter == button.name ? { color: '#fff' } : {}]"
      @click="filterHandler(button.name)"
    >
      {{ button.title }}
    </button>
  </div>
</template>

<script>
export default {
  props: {
    updataFilter: { 
      typeof: Function,
      required: true,
    },
    activeFilter: {
      typeof: Function,
      required: true,
    },
  },
  data() {
    return {
      buttons: [
        {
          title: "Barcha kinolar",
          name: "all",
        },
        {
          title: "Mashhur kinolar",
          name: "popular",
        },
        {
          title: "Eng ko'p ko'rilgan kinolar",
          name: "mostViewers",
        },
      ],
      filter: "all",
    };
  },
  methods: {
    filterHandler(filter) {
      this.filter = filter;
      this.updataFilter(this.filter);
    },
  },
};
</script>

<style scoped></style>
