<template>
  <v-app>
    <v-container>
        <v-container>
          <v-text-field
            v-model="search"
            label="Pesquisar"
            placeholder="Bulbassaur" outlined
          ></v-text-field>
          <v-row>
            <v-col cols="3" v-for="pokemon in filtered_pokemons || filtered_species" :key="pokemon.name">
              <v-card>
                <v-container>
                  <v-row class="mx-0 d-flex justify-center">
                    <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(pokemon)}.png`" :alt="pokemon.name" width="80%">
                  </v-row>
                  <h2 class="text-center">{{get_name(pokemon)}}</h2>
                </v-container>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
    </v-container>
  </v-app>
</template>

<script>
// import HelloWorld from './components/HelloWorld';
import axios from "axios";
export default {
  name: "App",

  components: {},

  data() {
    return {
      pokemons: [],
      search: "",
    };
  },

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=151")
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },
  methods:{
    get_id(pokemon){
      return Number(pokemon.url.split("/")[6]);
    },
    get_name(pokemon){
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },
  },
  computed:{
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    },
     filtered_species() {
      return this.pokemons.filter((item) => {
        return item.species.includes(this.search);
      });
    },
  },
};
</script>

<style>
#app {
background: rgb(12,39,63);
background: radial-gradient(circle, rgba(12,39,63,1) 49%, rgba(10,10,10,1) 100%)
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}
</style>