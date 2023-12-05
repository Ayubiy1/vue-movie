e
<template>
  <div class="app">
    <div class="contant">
      <AppInfo
        :allCountMovies="movies.length"
        :favouriteMoviesCount="movies.filter((f) => f.favourite).length"
      />

      <div class="search-panel">
        <SearchPanel @searchMovie="searchMovie" />
        <AppFilter :updataFilter="updataFilter" :activeFilter="filter" />
      </div>

      <div
        v-if="movies.length < 1 && isLoading === false"
        style="
          display: flex;
          align-items: center;
          justify-content: center;
          padding: 1.5rem;
          background-color: #fcfaf5;
          border-radius: 4px;
          box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
        "
      >
        <h2>Kinolat topilmadi!</h2>
      </div>
      <div v-else-if="isLoading === true">
        <h2
          style="
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 1.5rem;
            background-color: #fcfaf5;
            border-radius: 4px;
            box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
          "
        >
          Loading...
        </h2>
      </div>

      <MovieList
        v-else
        :filterValue="term"
        :movies="onFileter(onSearch(movies, term), filter)"
        @onToggle="onToggle"
        @onRevome="onRevome"
      />

      <div
        style="
          display: flex;
          align-items: center;
          justify-content: center;
          padding: 1.5rem;
          background-color: #fcfaf5;
          border-radius: 4px;
          box-shadow: 15px 15px 15px rgba(0, 0, 0, 0.15);
        "
      >
        <nav aria-label="pagination">
          <ul class="pagination pagination-sm">
            <li
              v-for="pageNumber in totalPage"
              key="pageNumber"
              :class="[pageNumber == this.page ? 'active' : '']"
              @click="changePage(pageNumber)"
              style="cursor: pointer"
            >
              <span class="page-link">{{ pageNumber }}</span>
            </li>
          </ul>
        </nav>
      </div>

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
      movies: [],
      term: "",
      filter: "all",
      isLoading: false,
      page: 1,
      dataLimit: 10,
      totalPage: 0,
    };
  },

  methods: {
    async createMovie(item) {
      try {
        this.isLoading = true;
        const res = await axios.post(
          "https://jsonplaceholder.typicode.com/posts",
          item,
        );
        this.movies.push(res.data);
        this.isLoading = false;
      } catch (e) {
        alert(e.message);
      }
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
    async onRevome(id) {
      try {
        this.isLoading = true;
        const res = await axios.delete(
          `https://jsonplaceholder.typicode.com/posts/${id}`,
        );

        this.movies = this.movies.filter((item) => item.id !== id);
        this.isLoading = false;
      } catch (e) {
        alert(e.message);
      }
    },

    searchMovie(value) {
      this.term = value;
    },
    onSearch(movies, term) {
      if (term.length == 0) {
        return movies;
      } else {
        return movies.filter(
          (item) => item.name.toLowerCase().indexOf(term.toLowerCase()) > -1,
        );
      }
    },

    onFileter(movies, filter) {
      switch (filter) {
        case "popular":
          return movies.filter((m) => m.like);
        case "mostViewers":
          return movies.filter((m) => m.viewrs > 100);
        default:
          return movies;
      }
    },

    updataFilter(filter) {
      this.filter = filter;
    },

    async fetchMovies() {
      this.isLoading = true;
      try {
        setTimeout(async () => {
          const res = await axios.get(
            "https://jsonplaceholder.typicode.com/posts",
            {
              params: {
                _limit: this.dataLimit,
                _page: this.page,
              },
            },
          );
          const newArr = res.data.map((n) => {
            return {
              id: n.id,
              name: n.title,
              like: false,
              favourite: false,
              viewrs: n.id * 15,
            };
          });

          this.totalPage = Math.ceil(
            res.headers["x-total-count"] / this.dataLimit,
          );
          this.movies = newArr;
          this.isLoading = false;
        }, 1000);
      } catch (error) {
        console.log(error);
      }
    },

    changePage(pageNumber) {
      this.page = pageNumber;
    },
  },

  mounted() {
    this.fetchMovies();
  },

  watch: {
    page() {
      this.fetchMovies();
    },
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
