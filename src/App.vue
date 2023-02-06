<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
    <a href="/"><img id="img-logo" src="./assets/guia.png"></a>
    <hr>
    <h4 class="is-size-4">Pokedex</h4>
    <input type="text" class="input is-rounded" placeholder="Buscar pokemon pelo nome" v-model="search">
    <button id="searchBtn" @click="searchPokemon" class="button is-fullwidth is-success">Buscar</button>
      <div v-for="(pokemon, index) in filteredPokemons" :key="pokemon.url">
        <Pokemon :name="pokemon.name" :url="pokemon.url" :num="index+1"/>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon.vue';

const apiUrl = "https://pokeapi.co/api/v2/"

export default {
  name: 'App' ,
  data(){
    return{
      pokemons: [],
      filteredPokemons: [],
      search: ''
    }
  },
  created: function(){
    axios.get(apiUrl+"pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },
  methods:{
    searchPokemon: function(){
      this.filteredPokemons = this.pokemons;
      if(this.search == '' || this.search == ' '){
        this.filteredPokemons = this.pokemons;
      } else {
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.search)      
        
        if(this.filteredPokemons.length == 0){
          //this.search == "";
          this.filteredPokemons = this.pokemons;
        }  
      }
    }
  },
  computed: {
    // searchResult: function(){
    //   if(this.search == '' || this.search == ' '){
    //     return this.pokemons;
    //   } else {
    //     return this.pokemons.filter(pokemon => pokemon.name == this.search)
    //   }
    // }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#img-logo{
  background-color: blue;
}
#searchBtn{
  margin-top: 2%;
}
</style>
