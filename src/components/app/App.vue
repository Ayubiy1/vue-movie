<template>
  <div class="app">
    <div class="contant">
      <AppInfo
        :allCountMovies="movies.length"
        :favouriteMoviesCount="movies.filter((f) => f.favourite).length"
      />

      <button @click="fetchMovies">Click</button>

      <div class="search-panel">
        <SearchPanel @searchMovie="searchMovie" />
        <AppFilter :updataFilter="updataFilter" :activeFilter="filter" />
      </div>

      <MovieList
        :filterValue="term"
        :movies="onFileter(onSearch(movies, term), filter)"
        @onToggle="onToggle"
        @onRevome="onRevome"
      />

      <MovieAddFrom :movies="movies" @createMovie="createMovie" />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import AppInfo from "@/components/app-info/AppInfo.vue";
import SearchPanel from "../search-panel/SearchPanel.vue";
import AppFilter from "../app-filter/AppFilter.vue";
import MovieList from "../movie-list/MovieList.vue";
import MovieAddFrom from "../movie-add-from/MovieAddFrom.vue";
export default {
  components: {
    AppInfo,
    SearchPanel,
    AppFilter,
    MovieList,
    MovieAddFrom,
  },

  data() {
    return {
      movies: [
        {
          name: "Movie one",
          viewrs: 124,
          favourite: true,
          like: false,
          id: 1,
        },
        {
          name: "Movie two",
          viewrs: 1231,
          favourite: true,
          like: true,
          id: 2,
        },
        {
          name: "Movie three",
          viewrs: 412,
          favourite: false,
          like: false,
          id: 3,
        },
        {
          name: "Movie for",
          viewrs: 765,
          favourite: true,
          like: false,
          id: 4,
        },
      ],
      term: "",
      filter: "all",
    };
  },

  methods: {
    createMovie(item) {
      this.movies.push(item);
    },
    onToggle({ id, prop }) {
      this.movies = this.movies.map((item) => {
        if (id == item.id) {
          return { ...item, [prop]: !item[prop] };
        } else {
          return item;
        }
      });
    },
    onRevome(id) {
      console.log(id);
      this.movies = this.movies.filter((item) => item.id !== id);
    },

    searchMovie(value) {
      this.term = value;
    },
    onSearch(movies, term) {
      if (term.length == 0) {
        return movies;
      } else {
        return movies.filter(
          (item) => item.name.toLowerCase().indexOf(term.toLowerCase()) > -1
        );
      }
    },

    onFileter(movies, filter) {
      switch (filter) {
        case "popular":
          return movies.filter((m) => m.like);
        case "mostViewers":
          return movies.filter((m) => m.viewrs > 600);
        default:
          return movies;
      }
    },

    updataFilter(filter) {
      this.filter = filter;
    },

    unmountedLog() {
      console.log("unmounted");
    },

    async fetchMovies() {
      try {
        const { data } = await axios.get(
          "https://jsonplaceholder.typicode.com/users/1/albums?_limit_10"
        );
        const newArr = data.map((n) => {
          return {
            id: n.id,
            name: n.title,
            like: false,
            favourite: false,
            viewrs: n.id * 15,
          };
        });

        console.log(newArr);
      } catch (error) {
        console.log(error);
      }
    },
  },

  unmounted() {
    unmountedLog();
  },
};
</script>

<style>
.app {
  height: 100vh;
  color: black;
}
.contant {
  max-width: 1000px;
  min-height: 700px;
  background-color: white;
  margin: 0 auto;
  padding: 5rem 0;
}
.search-panel {
  margin-top: 2rem;
  padding: 1.5rem;
  background-color: #fcfaf5;
  border-radius: 4px;
  box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
}
</style>
