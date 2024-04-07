<template>
    <div id="pokemon">
      <div class="card">
        <div class="card-image">
          <figure>
            <img :src="currentImg" alt="Pokemon Image" />
          </figure>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">{{ formattedNum }} - {{ formattedName }}</p>
              <p class="subtitle is-6">{{ pokemon.type }}</p>
            </div>
          </div>
          <div class="content">
            <button class="button is-medium is-fullwidth" @click="toggleSprite">Toggle Sprite</button>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "PokemonCard",
    props: {
      num: Number,
      name: String,
      url: String,
    },
    data() {
      return {
        currentImg: "",
        pokemon: {
          type: "",
          front: "",
          back: "",
        },
        isFront: true,
      };
    },
    created() {
      this.fetchPokemonData();
    },
    methods: {
      fetchPokemonData() {
        axios
          .get(this.url)
          .then(response => {
            const data = response.data;
            this.pokemon.type = data.types[0].type.name;
            this.pokemon.front = data.sprites.front_default;
            this.pokemon.back = data.sprites.back_default;
            this.currentImg = this.pokemon.front;
          })
          .catch(error => {
            console.error("Erro ao obter o Pokémon:", error);
          });
      },
      toggleSprite() {
        this.isFront = !this.isFront;
        this.currentImg = this.isFront ? this.pokemon.front : this.pokemon.back;
      },
    },
    computed: {
      formattedName() {
        return this.name.charAt(0).toUpperCase() + this.name.slice(1);
      },
      formattedNum() {
        return this.num.toString().padStart(3, "0");
      },
    },
  };
  </script>
  
  <style>
  #pokemon {
    margin-top: 2%;
  }
  </style>
  