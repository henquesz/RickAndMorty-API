<template>
  <v-app>
    <v-container>
      <v-img
        :src="require('../src/assets/logoram.png')"
        class=""
        contain
        height="300"
      />
      <v-text-field
        v-model="search"
        dark
        label="Search Character"
        placeholder="Evil Morty"
        outlined
        color="green"
      ></v-text-field>
      <v-container>
        <v-row>
          <v-col
            cols="3"
            v-for="personagem in filtered_personagens"
            :key="personagem.name"
          >
            <v-card dark>
              <v-container>
                <!-- {{get_id(personagem)}} -->
                <v-row class="mx-0 d-flex justify-left">
                  <img
                    id="imagem"
                    :src="`https://rickandmortyapi.com/api/character/avatar/${get_id(
                      personagem
                    )}.jpeg`"
                    :alt="personagem.name"
                    width="50%"
                    class="text-center"
                  />
                  <v-container>
                    <h2 class="white--text">{{ personagem.name }}</h2>
                    <h4 class="white--text">
                      {{ personagem.status }} - {{ personagem.species }}
                    </h4>
                    <v-divider class="my-4"></v-divider>
                    <v-chip label>
                      <h6 class="grey--text">
                        Origin: {{ personagem.origin.name }}
                      </h6>
                    </v-chip>

                    <v-chip label style="margin-top:10px;">
                      <h6 class="grey--text">
                        Location: {{ personagem.location.name }}
                      </h6>
                    </v-chip>
                  </v-container>
                </v-row>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
      <div class="text-center">
    <v-pagination
      v-model="page"
      :length="6"
    ></v-pagination>
  </div>
    </v-container>
  </v-app>
</template>


<script>
let api = "https://rickandmortyapi.com/api/character/";
// import HelloWorld from './components/HelloWorld';
import axios from "axios";
export default {
  name: "App",

  components: {},

  data() {
    return {
      personagens: [],
      search: "",
    };
  },

  mounted() {
    axios
      //Local de url para substituir por rota da ApiRest desenvolvida. (https://github.com/henquesz/ApiRestSX)
      .get(api)
      .then((response) => {
        this.personagens = response.data.results;
        console.log(response);
      });
  },
  methods: {
    get_id(personagem) {
      return Number(personagem.url.split("/")[5]);
    },
    // pagination(personagem){

    // }
  },
  computed: {
    filtered_personagens() {
      return this.personagens.filter((item) => {
        return item.name.includes(this.search);
      });
    },
  },
};
</script>

<style>
#app {
  background: rgb(43, 43, 43);
}
#imagem {
  border-radius: 15px;
  padding-top: 10px;
}
</style>