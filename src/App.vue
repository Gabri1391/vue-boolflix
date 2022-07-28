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
     <div class="mt-5">
     <ul>
        <li class="p-2" v-for="movie in searchedContents" :key="movie.id">
          <strong>Title: {{ movie.title }}</strong>
          <ul>
            <li><strong>Original title:</strong> {{ movie.original_title }}</li>
            <li><strong>Language:</strong><img src="./assets/flags/en.png" alt="Country"></li>
            <li><strong>Vote:</strong> {{ movie.vote_average }}</li>
          </ul>
        </li>
     </ul>
     </div>
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
      searchedContents: []
    }
  },
  methods:{
    callApi(){
      if (!this.searchQuery) return;

      axios.get(`${this.baseUri}/search/movie/?api_key=${this.apiKey}&query=${this.searchQuery}`)
      .then((res) => {
        this.searchedContents = res.data.results;
        
      })
      
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
