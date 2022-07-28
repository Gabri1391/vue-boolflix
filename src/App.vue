<template>
 <div>
  <div class="container my-5 d-flex justify-content-between align-center">
     <div class="logo">
      BOOLFLIX
     </div>
     <div>
      <input type="text" v-model="searchQuery" placeholder="Insert a keyword...">
     <button @click="callApi" class="btn">SEARCH</button>
     </div>
  </div>
  <!-- Movies -->
     <div class="mt-5">
      <h1>Movies</h1>
     <ul>
        <li class="p-2" v-for="movie in searchedMovies" :key="movie.id">
          <strong>Title: {{ movie.title }}</strong>
          <ul>
            <li><strong>Original title:</strong> {{ movie.original_title }}</li>
             <li>
              Lingua originale:
              {{ movie.original_language }}
              <img
                v-if="
                  movie.original_language === 'en' ||
                  movie.original_language === 'it'
                "
                :src="require(`./assets/flags/${movie.original_language}.png`)"
                alt="Country"
              />
            </li>
            <li><strong>Vote:</strong> {{ movie.vote_average }}</li>
          </ul>
        </li>
     </ul>
     </div>
     <!-- TVseries -->
     <h1>TVSeries</h1>
     <ul>
        <li v-for="TVSerie in searchedTV" :key="TVSerie.id">
          <strong>{{ TVSerie.name }}</strong>
          <ul>
            <li class="p-2">Original titke: {{ TVSerie.original_name }}</li>
            <li>
              Language:
              {{ TVSerie.original_language }}
              <img
                v-if="
                  TVSerie.original_language === 'en' ||
                  TVSerie.original_language === 'it'
                "
                :src="
                  require(`./assets/flags/${TVSerie.original_language}.png`)
                "
                :alt="TVSerie.original_language"
              />
            </li>
            <li>Vote: {{ TVSerie.vote_average }}</li>
          </ul>
        </li>
      </ul>
     

 </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data(){
    return{
      searchQuery: "",
      baseUri:"https://api.themoviedb.org/3",
      apiKey: "ef9005034e403e3d4179e8aa9211d0a3",
      searchedMovies: [],
      searchedTV: []
    }
  },
  methods:{
    callApi(){
      if (!this.searchQuery) return;

      axios.get(`${this.baseUri}/search/movie/?api_key=${this.apiKey}&query=${this.searchQuery}`)
      .then((res) => {
        this.searchedMovies = res.data.results;
        
      });

      axios.get(`${this.baseUri}/search/tv/?api_key=${this.apiKey}&query=${this.searchQuery}`)
      .then((res) => {
        this.searchedTV = res.data.results;
      } );
      
      this.searchQuery = "";
    }
  }
}
</script>

<style lang="scss">
@import './assets/scss/style.scss';

img{
  width: 30px;
  padding: 5px;
}

</style>
