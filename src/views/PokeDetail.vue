<template>
  <v-container >
    <div class="spinner" v-show="loading">
      <clip-loader :loading="loading" class="spinner__element" :color="'#68d391'" :size="60" />
    </div>

    <div v-if="!loading">
      <h2 :class="'capitalize'" class="my-2">Nom: {{pokemonDetail.name}}</h2>
    </div>

    <v-row class="mb-4" v-if="!loading">
      <v-col cols="12" md="6" class="d-flex justify-center" >
        <v-img
          height="220"
          max-width="200"
          class="blue-grey lighten-4 rounded-circle mt-4"
          contain
          :src="pokemonDetail.sprites.front_default"
          alt="Pokemón"
          transition="fab-transition"
          :class="'shadow'"
          
        >
        </v-img>

      </v-col>
      <v-col cols="12" md="6" class="d-flex justify-center">
        <v-img
          height="220"
          max-width="200"
          class="blue-grey lighten-4 rounded-circle mt-4"
          contain
          :src="pokemonDetail.sprites.back_default ? pokemonDetail.sprites.back_default : ImageNotFound"
          alt="Pokemón"
          transition="fab-transition"
          :class="'shadow'"
        >
        </v-img>

      </v-col>
    </v-row>
    <v-divider v-if="!loading" class="mt-7"></v-divider>
    
    <div v-if="!loading">
      <h2 :class="'capitalize'" class="my-5">Les caractéristiques</h2>
      <p>Height: <strong class="purple--text lighten-1">{{ pokemonDetail.height /10 }} m</strong> </p>
      <p>Weight: <strong class="purple--text lighten-1">{{ pokemonDetail.weight / 10 }} Kg</strong></p>
      <p>Type: <strong :class="'capitalize'" class="purple--text lighten-1">{{ typeMap[0].type.name }}</strong></p>
      <v-divider class="mt-6"></v-divider>
    </div>

    <div v-if="!loading">
      <h2 :class="'capitalize'" class="mt-5">Ability</h2>
      <list-abilities class="my-3" :abilities="abalitiesMap" />
      <v-divider class="mt-4"></v-divider>
    </div>

    
  </v-container>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import ListAbilities from '@/components/ListAbilities.vue'


export default {
  name: 'PokeDetail',
  components: { ListAbilities },
  data: () => ({
    loading: false,
    ImageNotFound: 'https://bitsofco.de/content/images/2018/12/broken-1.png'
  }),
  computed: {
    ...mapState(['pokemonDetail']),
    abalitiesMap () {
      return this.pokemonDetail.abilities.map(item => item)
    },
    typeMap () {
      return this.pokemonDetail.types.map(item => item)
    }
    
  },
  methods: {
    ...mapActions(['detailPokemon'])
  },
  created () {
    const id = this.$route.params.pokemon
    this.loading = true
    this.detailPokemon(id).finally(() => this.loading = false)
  }

}
</script>

<style lang="scss" scoped>
  .capitalize {
    text-transform: capitalize;
    @media only screen and (max-width: 586px) {
      text-align: center;
    }
    
  }
  .shadow {
    box-shadow: 0px 2px 3px 3px rgba(0,0,0,0.45)
  }
  .spinner {
    height: 75vh;
    display: flex;
    &__element {
      margin: auto;
    }
  }

</style>
