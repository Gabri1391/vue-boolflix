<template>
  <!-- Header -->
  <div>
    <header class="d-flex justify-content-between align-center">
      <div class="logo ps-5 text-danger fw-bold">BOOLFLIX</div>
      <div class="p-5">
        <input
          class="mt-5"
          type="text"
          @keyup.enter="callApi"
          v-model="searchQuery"
          placeholder="Insert a keyword..."
        />
        <button @click="callApi" class="bg-secondary text-white mt-5">SEARCH</button>
      </div>
    </header>
    <!-- Movies -->
    <div class="mt-5">
      <h1 class="ps-5 pb-2 text-danger fw-bold" v-show="isActive">Movies</h1>
      <ul class="row ps-4">
        <li
          class="p-3 col-12 col-md-4 col-lg-3 main-side"
          v-for="movie in searchedMovies"
          :key="movie.id"
        >
          <div>
            <strong> {{ movie.title }}</strong>
          </div>
          <img
            class="cover"
            :src="`https://image.tmdb.org/t/p/w342/${movie.poster_path}`"
            :alt="`${movie.title}`"
          />
          <ul class="overview bg-black text-white">
            <li><strong>Original title:</strong> {{ movie.original_title }}</li>
            <li>
              <strong>Original language:</strong>
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
            <li class="pt-1 plot">
              <strong>Plot: {{ movie.overview }}</strong>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <!-- TVseries -->
    <h1 class="ps-5 pb-2 text-danger fw-bold" v-show="isActive">TVSeries</h1>
    <ul class="row ps-4">
      <li
        class="p-3 col-12 col-md-4 col-lg-3 main-side"
        v-for="TVSerie in searchedTV"
        :key="TVSerie.id"
      >
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
            <strong>Original language:</strong>
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
          <i
            v-for="(star, i) in 5"
            :key="star.id"
            class="bi bi-star-fill"
            :class="{
              'text-warning': i < Math.ceil(movie.vote_average / 2),
            }"
          ></i>
          <li class="pt-1 plot">
            <strong>Plot: {{ movie.overview }}</strong>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
const coverPlaceholder =
  "https://www.ordingbo.it/wp-content/plugins/lightbox/images/No-image-found.jpg";
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
  computed: {
    cover() {
      if (!this.movie.poster_path) return coverPlaceholder;
      else return this.movie.poster_path;
    },
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
header {
  height: 150px;
  background-color: black;

  .logo {
    font-size: 2rem;
    line-height: 150px;
  }
}

.country-flag {
  width: 30px;
  padding: 5px;
}
.cover {
  width: 200px;
}

.main-side {
  position: relative;

  &:hover {
    .overview {
      display: block;
      position: absolute;
      top: 40px;
      left: 15px;
      transition: 2s linear;
      width: 200px;
      overflow: auto;
      transition: all 0.8s;
      opacity: 80%;

      .plot {
        font-size: 14px;
      }
    }
  }
  .overview {
    display: none;
  }
}
</style>
