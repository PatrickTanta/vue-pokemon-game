<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>

    <div v-else>
        <h1>¿Quién es este pokémon?</h1>

        <PokemonPicture
            :pokemon-id="pokemon.id"
            :show-pokemon="showPokemon"
        />

        <PokemonOptions
            :pokemons="pokemonArr"
            @selection-pokemon="checkAnswer"
        />

        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">
                Nuevo Juego
            </button>
        </template>

    </div>

</template>

<script setup>
import { onMounted, ref } from 'vue';
import PokemonOptions from '@/components/PokemonOptions'
import PokemonPicture from '@/components/PokemonPicture'
import getPokemonOptions from '@/helpers/getPokemonOptions'

const pokemonArr = ref([])
const pokemon = ref(null)
const showPokemon = ref(false)
const showAnswer = ref(false)
const message = ref('')

onMounted(() => {
    mixPokemonArray()
})

const mixPokemonArray = async () => {
    pokemonArr.value = await getPokemonOptions()

    const rndInt = Math.floor( Math.random() * 4 )
    pokemon.value = pokemonArr.value[ rndInt ]
}

const checkAnswer = ( selectedId ) => {
    showPokemon.value = true
    showAnswer.value  = true

    if( selectedId === pokemon.value.id ) {
        message.value = `Correcto, ${ pokemon.value.name }`
    } else {
        message.value = `Oops, era ${ pokemon.value.name }`
    }
}

const newGame = () => {

    showPokemon.value = false
    showAnswer.value  = false
    pokemonArr.value  = []
    pokemon.value     = null
    mixPokemonArray()

}
</script>
