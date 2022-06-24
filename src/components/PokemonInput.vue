<template>
  <div>
    <label>Nombre:</label>
    <input
        type="text"
        name="pokemon"
        v-model:name="namePokemon"
        @keyup.enter="funcApi"
    />
    <button @click.prevent="funcApi">Buscar</button>
    <br>
    <h3>{{ pokeName }}</h3>
    <img :src="pokeImage" alt="Imagen pokemon" class="fade-in">


    <div class="options-container">
      <h3>Movimientos</h3>
      <ul>
        <li v-for="(move, index) in movesPokemon" :key="index">{{ move.move.name }}</li>
      </ul>

      <h3>Habilidades</h3>


      <ul>
        <li v-for="(ability, index) in abilityPokemon" :key="index">{{ ability.ability.name }}</li>
      </ul>
    </div>
  </div>
</template>

<script>

import pokemonApi from '@/api/pokemonApi'

export default {
  name: "PokemonInput",
  data() {
    return {
      namePokemon: "",
      pokemon: {
        name: "",
        sprites: {
          other: {
            dream_world: {
              front_default: ""
            }
          }
        },
        moves: [],
        abilities: [],
      },
    };
  },
  methods: {
    async funcApi() {
      console.log(this.url);
        const resp = await pokemonApi.get(this.url)
        console.log(resp);
        try {
          this.pokemon = resp.data;
          this.namePokemon = "";
        } catch (error) {
          console.log(`Se produjo un error en la conexión: \n${error}`);
        }
   },

    // ApiPokemon() {
    //   fetch(this.url)
    //       .then((res) => res.json())
    //       .then((data) => {
    //        // console.log(data);
    //         this.pokemon = data;
    //         this.namePokemon = "";
    //       })
    //       .catch((error) => console.log(`Se produjo un error en la conexión: \n${error}`));
    // },
  },
  created() {
    //this.ApiPokemon();
    this.funcApi()
  },
  computed: {
    // url() {
    //   return this.namePokemon.trim() === ""
    //       ? `https://pokeapi.co/api/v2/pokemon/pikachu`
    //       : `https://pokeapi.co/api/v2/pokemon/${this.namePokemon.toLowerCase().trim()}`;
    // },
    url() {
      return this.namePokemon.trim() === "" ? 'pikachu' : this.namePokemon.toLowerCase().trim()
    },
    pokeName() {
      return this.pokemon.name.toUpperCase();
    },
    pokeImage() {
      //  console.log(this.pokemon.sprites.other.dream_world.front_default);
      return this.pokemon.sprites.other.dream_world.front_default;

    },
    movesPokemon() {
      return this.pokemon.moves;
    },
    abilityPokemon() {
      return this.pokemon.abilities;
    },
  }
}
</script>

<style scoped>
ul {
  list-style-type: none;
}

li {
  background-color: white;
  border-radius: 5px;
  border: 1px solid rgba(0, 0, 0, 0.2);
  cursor: pointer;
  margin-bottom: 10px;
  width: 250px;
}

li:hover {
  background-color: rgba(0, 0, 0, 0.05);
}

.options-container {
  display: flex;
  justify-content: center;
  gap: 20px;
}
</style>