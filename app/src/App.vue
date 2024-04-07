<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="https://raw.githubusercontent.com/PokeAPI/media/master/logo/pokeapi_256.png" alt="">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" class="input" placeholder="Search pokemon" v-model="busca">
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-if="resultadoBusca.length === 0">
        <p>Nenhum Pokémon encontrado.</p>
      </div>
      <div v-else>
        <div v-for="(poke, index) in resultadoBusca" :key="poke.url">
          <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  data() {
    return {
      pokemons: [],
      busca: ''
    };
  },
  created() {
    this.fetchPokemons();
  },
  components: {
    Pokemon
  },
  methods: {
    fetchPokemons() {
      axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0')
        .then(response => {
          console.log('Pegou a lista de pokemon');
          this.pokemons = response.data.results;
        })
        .catch(error => {
          console.error('Erro ao obter a lista de pokemon:', error);
        });
    },
    buscar() {
      const searchTerm = this.busca.trim().toLowerCase();
      if (!searchTerm) {
        this.resultadoBusca = this.pokemons;
        return;
      }
      this.resultadoBusca = this.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(searchTerm));
    }
  },
  computed: {
    resultadoBusca() {
      return this.busca ? this.pokemons.filter(pokemon => pokemon.name.toLowerCase().includes(this.busca.trim().toLowerCase())) : this.pokemons;
    }
  }
};
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

#buscaBtn {
  margin-top: 2%;
}
</style>
