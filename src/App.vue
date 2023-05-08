<template>
  <v-app>
    <v-container>
      <v-row class="mt-1">
        <img :src="require(`@/assets/Pokemon.png`)"/>
      </v-row>
    </v-container>
    <v-container>
      <v-container>
        <v-text-field
          v-model="search"
          label="Pesquisar"
          placeholder="Charmander"
          solo
        >
        </v-text-field>

        <v-row>
          <v-col
            md="2"
            v-for="pokemon in filtered_pokemons"
            :key="pokemon.name"
          >
            <v-card class="pokemon" @click="show_pokemon(get_id(pokemon))">
              <v-container>
                <v-row class="mx-0 d-flex justify-content-center">
                  <img
                    :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${get_id(
                      pokemon
                    )}.png`"
                    :alt="pokemon.name"
                    width="200px"
                  />
                </v-row>
                <h2 class="text-center">{{ get_name(pokemon) }}</h2>
              </v-container>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-container>

    <v-dialog v-model="show_dialog" width="800">
      <v-card v-if="selected_pokemon">
        <v-container>
          <h1 class="text-center mt-1">{{ get_name(selected_pokemon) }}</h1>
          <v-row class="d-flex align-center">
            <v-col cols="5">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${selected_pokemon.id}.png`"
                :alt="selected_pokemon.name"
                width="80%"
              />
            </v-col>
            <v-col cols="7">
              <v-container>
                <v-row>
                  <v-container>
                    <h3>Tipos de pokemon</h3>
                  </v-container>
                  <v-container class="tipos">
                    <v-chip
                      label
                      v-for="type in selected_pokemon.types"
                      :key="type.slot"
                      class="mr-2"
                    >
                      {{ type.type.name }}
                    </v-chip>
                  </v-container>
                </v-row>
              </v-container>
              <v-container class="mt-2">
                <v-row>
                  <v-container>
                    <h3>Características do pokemon</h3>
                  </v-container>
                  <v-container>
                    <v-chip label class="características">
                      Altura: {{ selected_pokemon.height / 10 }} cm
                    </v-chip>
                  </v-container>
                  <v-container class="características">
                    <v-chip label>
                      Peso: {{ (selected_pokemon.weight / 10) }} Kg
                    </v-chip>
                  </v-container>
                  <v-container>
                    <v-chip label class="características">
                      Experiência Base: {{ selected_pokemon.base_experience }} XP
                    </v-chip>
                  </v-container>
                </v-row>
              </v-container>
            </v-col>
          </v-row>

          <h2 class="text-center titulos">Movimentos</h2>
          <h3 class="text-center" v-if="filter_moves(selected_pokemon).length==0">Esse pokemon não possue nenhum movimento ainda</h3>
          <v-simple-table class="text-center" v-if="filter_moves(selected_pokemon).length>0">
            <template>
              <thead>
                <tr>
                  <th class="text-center">Level</th>
                  <th class="text-center">Nome</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="item in filter_moves(selected_pokemon)"
                  :key="item.move.name"
                >
                  <td>{{ get_move_level(item) }}</td>
                  <td>{{ item.move.name }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>

          <h2 class="text-center titulos">Variações</h2>
          <v-row class="d-flex align-center">
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/shiny/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/shiny/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/home/shiny/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/shiny/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
          </v-row>

          <h2 class="text-center titulos">Versões</h2>
          <v-row class="d-flex align-center">
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/red-blue/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-i/yellow/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/crystal/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/gold/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-ii/silver/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/emerald/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/firered-leafgreen/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iii/ruby-sapphire/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/diamond-pearl/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/heartgold-soulsilver/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-iv/platinum/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vi/omegaruby-alphasapphire/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vi/x-y/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vii/icons/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-vii/ultra-sun-ultra-moon/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
            <v-col cols="4">
              <img
                :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-viii/icons/${selected_pokemon.id}.png`"
                width="200px"
              />
            </v-col>
          </v-row>

          <h2 class="text-center titulos">Estatísticas</h2>
          <v-container class="d-flex align-center">
            <v-container>
              <h4>{{ selected_pokemon.stats[0].stat.name }}: </h4>{{ selected_pokemon.stats[0].base_stat }}
            </v-container>
            <v-container>
              <h4>{{ selected_pokemon.stats[1].stat.name }}: </h4>{{ selected_pokemon.stats[1].base_stat }}
            </v-container>
            <v-container>
              <h4>{{ selected_pokemon.stats[2].stat.name }}: </h4>{{ selected_pokemon.stats[2].base_stat }}
            </v-container>
            <v-container>
              <h4>{{ selected_pokemon.stats[3].stat.name }}: </h4>{{ selected_pokemon.stats[3].base_stat }}
            </v-container>
            <v-container>
              <h4>{{ selected_pokemon.stats[4].stat.name }}: </h4>{{ selected_pokemon.stats[4].base_stat }}
            </v-container>
            <v-container>
              <h4>{{ selected_pokemon.stats[5].stat.name }}: </h4>{{ selected_pokemon.stats[5].base_stat }}
            </v-container>
          </v-container>
        </v-container>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  components: {},

  data: () => ({
    pokemons: [],
    search: "",
    show_dialog: false,
    selected_pokemon: null,
  }),

  mounted() {
    axios
      .get("https://pokeapi.co/api/v2/pokemon?limit=102")
      .then((response) => {
        this.pokemons = response.data.results;
      });
  },

  methods: {
    get_id(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },

    get_name(pokemon) {
      return pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
    },

    show_pokemon(id) {
      axios.get(`https://pokeapi.co/api/v2/pokemon/${id}`).then((response) => {
        this.selected_pokemon = response.data;
        this.show_dialog = !this.show_dialog;
      });
    },

    get_move_level(move) {
      for (let version of move.version_group_details) {
        if (
          version.version_group.name == "sword-shield" &&
          version.move_learn_method.name == "level-up"
        ) {
          return version.level_learned_at;
        }
      }
    },

    filter_moves(pokemon) {
      return pokemon.moves.filter((item) => {
        let include = false;
        for (let version of item.version_group_details) {
          if (
            version.version_group.name == "sword-shield" &&
            version.move_learn_method.name == "level-up"
          ) {
            include = true;
          }
        }
        return include;
      });
    },
  },

  computed: {
    filtered_pokemons() {
      return this.pokemons.filter((item) => {
        return item.name.includes(this.search);
      });
    },
  },
};
</script>

<style>
#app {
  background: linear-gradient(
      to bottom right,
      rgba(10, 10, 10, 1),
      rgba(12, 39, 63, 1)
    )
    no-repeat center center fixed !important;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}

.pokemon {
  min-height: 160px;
  max-height: 354px;
}

.titulos {
  margin-top: 46px;
}

.tipos, .características {
  margin-top: -22px;
}
</style>