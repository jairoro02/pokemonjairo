<template >
    <h1>Puntuacion: {{this.puntuacion}}</h1>
    <h1>Vidas: {{this.vidas}}</h1>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else="pokemon">
        <h1>¿Quien es este pokemon?</h1>

        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />

        <PokemonOptions v-if="showOptions" :pokemons="pokemonsArr" @selection="checkAnswer" />
        <template v-if="perdedor">
            <h2 class="fade-in">{{ message }}</h2>
            <button style="width:250px; background-color:red;color:white;font-size:40px" @click="newGame">Has perdido</button>
        </template>
        <template v-if="showAnswer">
        <h2 class="fade-in">{{ message }}</h2>
        <button @click="newPokemon">Jugar de nuevo</button>
        </template>
        
    </div>
</template>

<script>
import PokemonOptions from '../components/PokemonOptions.vue'
import PokemonPicture from '../components/PokemonPicture.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'
export default {
  components: { PokemonOptions, PokemonPicture },
  data() {
    return {
      pokemonsArr: [],
      puntuacion:0,
      vidas:5,
      perdedor:false,
      showOptions: true,
      pokemon: null,
      showPokemon: false,
      showAnswer: false,
      message: '',
    }
  },
  methods: {
    async mixPokemonArray() {
      this.pokemonsArr = await getPokemonOptions()
      const rndInt = Math.floor(Math.random() * 4)
      this.pokemon = this.pokemonsArr[rndInt]
    },
    checkAnswer(selectedId) {
        this.showOptions = false
      this.showPokemon = true
      this.showAnswer = true
      if (selectedId === this.pokemon.id) {
        this.message = `Correcto, ${this.pokemon.name}`,
        this.puntuacion ++
      } else {
        this.message = `Ups, era ${this.pokemon.name}`,
        this.vidas --
        if(this.vidas == 0){
            this.showAnswer=false
            this.perdedor = true
            this.vidas = 5
            this.puntuacion = 0
        }
      }
    },
    newPokemon() {
      ;(this.showPokemon = false),
        (this.showAnswer = false),
        (this.pokemonsArr = []),
        (this.showOptions = true),
        this.mixPokemonArray(),
        (this.pokemon = null)
    },
    newGame() {
      ;(this.perdedor = false),
      (this.showPokemon = false),
        (this.showAnswer = false),
        (this.pokemonsArr = []),
        (this.showOptions = true),
        this.mixPokemonArray(),
        (this.pokemon = null)
    }

  },
  mounted() {
    this.mixPokemonArray()
  },
}
</script>

<style></style>