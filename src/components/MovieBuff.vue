<template>
  <container>
    <h1>{{ msg }}</h1>
    <p>
      Type your desired movie or series name below to search, or take a look at your favourites!
    </p>
    <div>
      <input type="text" v-model="searchText" placeholder="Search...">
      <button @click="Search">Search</button>
      <button @click="Favourites">Favourites</button>
    </div>
    <div class="search-results">
      <ul>
        <li v-for="result in searchResult" :key="result.imdbID">
          <div>
            <div id="imageSize">
              <img alt="Poster" :src="result.Poster" id="logo">
            </div>
            <div>
              <p>{{ result.Title }} ({{ result.Year }})</p>
              <p>IMDB ID: {{ result.imdbID }}</p>
              <p>Type: {{ result.Type.charAt(0).toUpperCase() + result.Type.slice(1) }}</p>
              <button v-if="!isFavourited(result)" @click="Favourite(result)">Favourite</button>
              <button v-else @click="Unfavourite(result)">Unfavourite</button>             
            </div>
          </div>
        </li>
      </ul>
    </div>
  </container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'MovieBuff',
  props: {
    msg: String
  },
  data(){
    return {
      searchText:'',
      searchResult: [],
      apiUrl: 'https://www.omdbapi.com/?s=',
      apiKey: '&apikey=3bef232e&',
      favourites: []
    }
  },
  methods: {
    async Search() {
      try{
        const response = await axios.get(this.apiUrl + this.searchText + this.apiKey)
        this.searchResult = response.data.Search;   
        console.log(response.data.Search)
      } catch (err) {
        console.error('Error fetching data:', err);
      }
    },
    Favourite(result) {
      console.log(result)
      this.favourites.push(result)
    },
    Unfavourite(result) {
      this.favourites = this.favourites.filter(favourite => favourite.imdbID !== result.imdbID)
    },
    isFavourited(result) {
      return this.favourites.some(favourite => favourite.imdbID === result.imdbID)
    },
    Favourites() {
      this.searchResult = this.favourites
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.search-results {
  display: flex;
  flex-wrap: wrap;
}

.search-results li {
  flex-basis: calc(33.33% - 10px);
  margin: 5px;
}
</style>
