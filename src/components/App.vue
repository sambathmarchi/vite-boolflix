<script>
import { store } from './store.js';
import axios from 'axios';

import MovieCard from './MovieCard.vue';
import SeriesCard from './SeriesCard.vue';
import SearchMovie from './SearchMovie.vue';

export default {
  components: {
    MovieCard,
    SeriesCard,
    SearchMovie,
  },

  data() {
    return {
      store,
    }
  },

  mounted() {
    this.getMovies();
  },

  methods: {
    getMovies() {
      const movieOptions = {
        method: 'GET',
        url: "https://api.themoviedb.org/3/search/movie",
        params: {
          query: `${this.store.getInput}`,
          include_adult: 'false',
          language: 'en-US',
          page: '1',
          api_key: '32b190ae5a577c8da7d39b336677cf49'
        },
      };

      axios
        .request(movieOptions)
        .then(function (response) {
          console.log(response.data.results);
          store.movies = response.data.results;
        })
        .catch(function (error) {
          console.error(error);
        });


      const seriesOptions = {
        method: 'GET',
        url: "https://api.themoviedb.org/3/search/tv",
        params: {
          query: `${this.store.getInput}`,
          include_adult: 'false',
          language: 'en-US',
          page: '1',
          api_key: '32b190ae5a577c8da7d39b336677cf49'
        },
      };

      axios
        .request(seriesOptions)
        .then(function (response) {
          console.log(response.data.results);
          store.series = response.data.results;
        })
        .catch(function (error) {
          console.error(error);
        });

    }

  },
}


</script>

<template>
  <header>
    <SearchMovie @search="getMovies" />
  </header>

  <main>
    <MovieCard />
    <SeriesCard />
  </main>
</template>

<style scoped>
header {
  width: 100%;
  height: 3.5rem;
  background-color: #121212;
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 0;
  z-index: 999;
  padding: 2.2rem 1.8rem;
}

main {
  width: 95%;
  margin: 0 auto;
  overflow-y: auto;
  padding: 2.5rem 1rem 1rem 1rem;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

main::-webkit-scrollbar {
  display: none;
}
</style>