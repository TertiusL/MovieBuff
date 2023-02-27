<template>
  <container>
    <h1>{{ msg }}</h1>
    <p>
      Type your desired movie or series name below to search, or take a look at your favourites!
    </p>
    <div style="padding-bottom: 10px;">
      <input type="text" v-model="searchText" placeholder="Search...">
      <button @click="Search">Search</button>
      <button @click="Favourites">Favourites</button>
    </div>
    <div class="main">
      <table v-if="!viewFavourites">
        <tbody>
          <tr v-for="result in searchResult" :key="result.imdbID">
            <td>
              <div class="result-item">
                <div style="text-align: center;">
                  <img alt="Poster" :src="result.Poster" id="poster">
                </div>                
              </div>
            </td>
            <td>
              <div class="details">
                  <p>{{ result.Title }} ({{ result.Year }})</p>
                  <p>IMDB ID: {{ result.imdbID }}</p>
                  <p>Type: {{ result.Type.charAt(0).toUpperCase() + result.Type.slice(1) }}</p>
                  <button v-if="!isFavourited(result)" @click="Favourite(result)">Favourite</button>
                  <button v-else @click="Unfavourite(result)">Unfavourite</button>             
                </div>
            </td>            
          </tr>
        </tbody>
      </table>
      <table v-if="viewFavourites">
        <tbody>
          <tr v-for="result in favourites" :key="result.imdbID">
            <td>
              <div class="result-item">
                <div style="text-align: center;">
                  <img alt="Poster" :src="result.Poster" id="poster">
                </div>                
              </div>
            </td>
            <td>
              <div class="details">
                  <p>{{ result.Title }} ({{ result.Year }})</p>
                  <p>IMDB ID: {{ result.imdbID }}</p>
                  <p>Type: {{ result.Type.charAt(0).toUpperCase() + result.Type.slice(1) }}</p>
                  <button v-if="!isFavourited(result)" @click="Favourite(result)">Favourite</button>
                  <button v-else @click="Unfavourite(result)">Unfavourite</button>             
                </div>
            </td>            
          </tr>
        </tbody>
      </table>
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
      favourites: [],
      viewFavourites: false
    }
  },
  created(){
    const favourites = localStorage.getItem('favourites')
    console.log('Created: '+ favourites)
    if (favourites) {
      this.favourites = JSON.parse(favourites)
      console.log('Created if: '+ this.favourites)
    }
  },
  watch: {
    favourites: function(newFavourites) {
      localStorage.setItem('favourites', JSON.stringify(newFavourites))
      console.log('watch: '+ newFavourites)
    }
  },
  methods: {
    async Search() {
      this.viewFavourites = false
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
      this.viewFavourites = true
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
.main {
  display: flex;
  justify-content: center;
}
.search-results li {
  flex-basis: calc(33.33% - 10px);
  margin: 5px;
}
.result-item {
  align-items: center;
  margin-bottom: 10px;
}
#poster{
  width: 50%;
  height: 50%;
}
.poster img {
  max-width: 100%;
}

.details {
  flex: 1;
}
table {
  border-collapse: collapse;
}

th, td {
  border: 1px solid rgb(83, 82, 82);
  padding: 8px;
  text-align: left;
}

th {
  background-color: #acaaaa;
}
</style>
