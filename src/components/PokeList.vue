<template>
  <v-container>
    <div class="spinner" v-show="spinner">
      <grid-loader :loading="spinner" class="spinner__element" :color="'#1976D2'" :size="60" />
    </div>


    <v-row v-if="!spinner">
      <v-col cols="12" sm="6" md="6" lg="4" v-for="(pokemon, index) in pokeFilter" :key="index">

        <v-card outlined shaped class="mx-auto px-3" :max-width="270" >
          <v-img
            height="230"
            class="blue-grey lighten-5 rounded-circle mt-4"
            contain
            :src="pokeImage + pokemon.id + '.png'"
            alt="Pokemón"
            transition="fab-transition"
          ></v-img>

          <v-card-title class="justify-center" :class="'capitalize'">{{ pokemon.name }}</v-card-title>

          <v-divider class="mx-4 mb-3"></v-divider>

          <v-card-actions>
            <v-btn color="deep-purple lighten-2" class="mx-auto white--text px-4"  @click="clickDetail(pokemon.name)">
              Détailles
            </v-btn>
          </v-card-actions>
        </v-card>
        

      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  name: 'PokeList',
  data: () => ({
    search: '',
    pokeImage: 'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/'
  }),
  props: {
    spinner: Boolean,
    pokemons: Array
  },
  methods: {
    ...mapActions(['getPokemons', 'detailPokemon']),
    clickDetail (pokemon) {
      this.$router.push({ name: 'PokeDetail', params: {pokemon} })
    },

  },
  computed: {
    pokeFilter () {
      return this.pokemons.filter(poke => {
        return poke.name.toLowerCase().includes(this.search.toLowerCase()) ||
               poke.id.includes(this.search)
      })
    },
  },
  mounted () {
  },
};
</script>

<style lang="scss" scoped>
  .capitalize {
    text-transform: capitalize;
    text-align: center;
  }
  .spinner {
    height: 75vh;
    display: flex;
    &__element {
      margin: auto;
    }
  }
</style>