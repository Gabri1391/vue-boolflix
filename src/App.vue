<template>
  <div>
    <div class="container my-5 d-flex justify-content-between align-center">
      <div class="logo">BOOLFLIX</div>
      <div>
        <input
          type="text"
          @keyup.enter="callApi"
          v-model="searchQuery"
          placeholder="Insert a keyword..."
        />
        <button @click="callApi" class="btn">SEARCH</button>
      </div>
    </div>
    <!-- Movies -->
    <div class="mt-5">
      <h1 class="ps-5 pb-2" v-show="isActive">Movies</h1>
      <ul>
        <li class="p-2" v-for="movie in searchedMovies" :key="movie.id">
          <div>
            <strong>Title: {{ movie.title }}</strong>
          </div>
          <img
            class="cover"
            :src="`https://image.tmdb.org/t/p/w342/${movie.poster_path}`"
            :alt="`${movie.title}`"
          />
          <ul>
            <li><strong>Original title:</strong> {{ movie.original_title }}</li>
            <li>
              <strong>Lingua originale:</strong>
              {{ movie.original_language }}
              <img
                class="country-flag"
                v-if="
                  movie.original_language === 'en' || movie.original_language === 'it'
                "
                :src="require(`./assets/flags/${movie.original_language}.png`)"
                alt="Country"
              />
            </li>
            <li>
              <strong>Vote: </strong>
              <i
                v-for="(star, i) in 5"
                :key="star.id"
                class="bi bi-star-fill"
                :class="{
                  'text-warning': i < Math.ceil(movie.vote_average / 2),
                }"
              ></i>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <!-- TVseries -->
    <h1 class="ps-5 pb-2" v-show="isActive">TVSeries</h1>
    <ul>
      <li class="p-2" v-for="TVSerie in searchedTV" :key="TVSerie.id">
        <div>
          <strong>Title: {{ TVSerie.name }}</strong>
        </div>
        <img
          class="cover"
          :src="`https://image.tmdb.org/t/p/w342/${TVSerie.poster_path}`"
          :alt="`${TVSerie.name}`"
        />
        <ul>
          <li>
            <strong> title: {{ TVSerie.original_name }}</strong>
          </li>
          <li>
            <strong>Language:</strong>
            {{ TVSerie.original_language }}
            <img
              class="country-flag"
              v-if="
                TVSerie.original_language === 'en' || TVSerie.original_language === 'it'
              "
              :src="require(`./assets/flags/${TVSerie.original_language}.png`)"
              :alt="TVSerie.original_language"
            />
          </li>
          <li><strong>Vote:</strong></li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      searchQuery: "",
      baseUri: "https://api.themoviedb.org/3",
      apiKey: "ef9005034e403e3d4179e8aa9211d0a3",
      searchedMovies: [],
      searchedTV: [],
      isActive: false,
      stars: 5,
    };
  },
  methods: {
    callApi() {
      if (!this.searchQuery) return;

      axios
        .get(
          `${this.baseUri}/search/movie/?api_key=${this.apiKey}&query=${this.searchQuery}`
        )
        .then((res) => {
          this.searchedMovies = res.data.results;
          this.isActive = true;
        });

      axios
        .get(
          `${this.baseUri}/search/tv/?api_key=${this.apiKey}&query=${this.searchQuery}`
        )
        .then((res) => {
          this.searchedTV = res.data.results;
          this.isActive = true;
        });

      this.searchQuery = "";
    },
  },
};
</script>

<style lang="scss">
@import "./assets/scss/style.scss";

.country-flag {
  width: 30px;
  padding: 5px;
}
.cover {
  width: 100px;
}
</style>
