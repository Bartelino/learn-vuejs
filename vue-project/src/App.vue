<template>
  <h1 :style="{ color: count > 5 ? 'red' : 'green' }">Compteur : {{ count }}</h1>
  <div v-if="count >= 5">Bravo vous avez cliquer plus de 5 fois</div>
  <div v-else>Moins de 5 fois</div>
  <button v-on:click="increment">Incrémenter</button>
  <button @click="decrement">Décrementer</button>
  <hr>
  <button @click="sortMovies">Réorganiser</button>
  <form action="" @submit.prevent="addMovie">
    <input type="text" placeholder="Nouveau film" v-model="movieName"> 
    {{ movieName }}
    <button>Ajouter</button>
  </form>

  <ul>
    <li v-for="movie in movies" :key="movie">
      {{ movie }} <button @click="deleteMovie(movie)">Supprimer</button>
    </li>
  </ul>

</template>

<script setup>
import { ref } from 'vue'
const count = ref(0)
const movieName = ref('')

const movies = ref([
  'Matrix',
  'Lilo',
  'Titanic'
])

const increment = () => {
  count.value++
}

const decrement = () => {
  count.value--
}


const deleteMovie = (movie) => {
  movies.value = movies.value.filter(m => m !== movie)
}

const sortMovies = () => {
  movies.value.sort((a, b) => a > b ? 1 : -1)
}

const addMovie = () => {
  movies.value.push(movieName.value)
  movieName.value = ""
}

</script>

<style>
h1 {
  color: rgb(73, 53, 53);
}
</style>