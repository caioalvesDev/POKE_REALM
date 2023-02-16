<template>
  <div id="app">
   
    <div class="column is-half is-offset-one-quarter">
      <div id="img">
        <img src="./assets/pokemon-png-logo.webp"  alt="">
      </div>
      <input class="input is-rounded" type="text" placeholder="Buscar Pokemon pelo nome" v-model="busca">
      <button class="button  is-fullwidth is-success" id="busca-btn" @click="buscar">Buscar Pokemon</button>
      <div v-for="(poke, index) in filteredpokemons" :key="poke.url">
          <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
    </div>
   
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/pokemon.vue';

export default {
  name: 'App',
  data(){
    return {
      pokemons: [],
      filteredpokemons: [],
      busca: ''
    }
  },
  created: function () {
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151$offset=0').then( response => {
      console.log(response.data.results);
      this.pokemons = response.data.results;
      this.filteredpokemons = response.data.results;
    })
  },
  components:{
    Pokemon
  },
  methods: {
    buscar: function() {
      this.filteredpokemons = this.pokemons;
      if (this.busca == '' || this.busca == ' ') {
          this.filteredpokemons = this.pokemons;
        } else {
          const buscar =  this.busca
         
          this.filteredpokemons = this.pokemons.filter(pokemon => pokemon.name.toUpperCase() == buscar.toUpperCase());
        }
    }
  },
  computed: {
    // resultadoBusca: function () {
    //   if (this.busca == '' || this.busca == ' ') {
    //     return this.pokemons;
    //   }else {
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca)
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
#busca-btn{
  margin-top: 2%;
}
#img{
  margin-bottom: 2%;
}
</style>
