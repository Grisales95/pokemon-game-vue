<template>
  <h1 v-if="!pokemon">Cargando...</h1>
  <div v-else>
    <h1>¿Quien es este pokémon?</h1>
    <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
    <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer" />

    <template v-if="showAnswer">
      <h2 class="fade-in">{{ msg }}</h2>

      <button @click="newGame">Nuevo juego</button>
    </template>
  </div>
</template>

<script>
import PokemonOptions from '../components/PokemonOptions';
import PokemonPicture from '../components/PokemonPicture';

import getPokemonOptions from '@/helpers/getPokemonOptions';

// console.log(getPokemonOptions());

export default {
  components: {
    PokemonPicture,
    PokemonOptions,
  },

  data() {
    return {
      pokemonArr: [],
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      msg: '',
    };
  },

  methods: {
    async mixPokemonArray() {
      this.pokemonArr = await getPokemonOptions();

      const rndInt = Math.floor(Math.random() * 4);

      this.pokemon = this.pokemonArr[rndInt];
    },

    checkAnswer(selectedId) {
      this.showPokemon = true;
      this.showAnswer = true;
      if (selectedId === this.pokemon.id) {
        this.msg = `Correcto, ${this.pokemon.name} `;
      } else {
        this.msg = `Ops, era ${this.pokemon.name}`;
      }
    },

    newGame() {
      this.pokemonArr = [];
      this.pokemon = null;
      this.showPokemon = false;
      this.showAnswer = false;

      this.mixPokemonArray();
    },
  },

  mounted() {
    this.mixPokemonArray();
  },
};
</script>
