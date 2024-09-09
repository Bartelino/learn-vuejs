<template>
    <button @click="showTimer = !showTimer">Afficher / Masquer</button>
    <Timer v-if="showTimer"></Timer>
    <Layout>
        <template #header>
            En tête
        </template>
        <template v-slot:aside>
            Coté
        </template>
        <template v-slot:main>
            Main
        </template>
        <template v-slot:footer>
            Footer
        </template>
    </Layout>
    <Button><strong>Test</strong> ici</Button>
    <form action="" @submit.prevent="addTodo">
        <fieldset role="group">
            <input type="text" placeholder="Tâche à effectuer" v-model="newTodo">
            <button :disabled="newTodo.length == 0">Ajouter</button>
        </fieldset>
    </form>
    <div v-if='todos.length == 0'>Il n'y a pas de tâche.</div>
    <div v-else>
        <ul>
            <li v-for="todo in sortedTodo" :key="todo.date" :class="{ completed: todo.completed }">

                <Checkbox :label="todo.title" v-model="todo.completed" />
            </li>
        </ul>

        <label>
            <input type="checkbox" v-model="hideCompleted">
            Masquer les tâches complétées
        </label>
        <p v-if="remainingTodos > 0"> {{ remainingTodos }} tâche{{ remainingTodos > 1 ? 's' : '' }} à faire.</p>

        <Checkbox label="Bonjour" @check="(p) => console.log('coché', p)" />
    </div>

    <!--

Avant correction
      <form action="" @submit.prevent="addtache">
        <input type="text" placeholder="Ajouter une tâche." v-model="newtache">
        <button>Ajouter</button>
    </form>
    <div v-if='taches.length === 0'>Il n'y a pas de tâche.</div>
    <ul>
        <li v-for="tache in taches" :key="tache">
            {{ tache.date }} - {{ tache.title }} <button @click="completed(tache)">V</button>
        </li>
    </ul>
-->
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
import Checkbox from './checkbox.vue'
import Button from './button.vue'
import Layout from './layout.vue'
import Timer from './timer.vue'

onMounted(() => {
    fetch('https://jsonplaceholder.typicode.com/todos')
        .then(r => r.json())
        .then(v => todos.value = v.map(todo => ({ ...todo, date: todo.id })))
})

const hideCompleted = ref(false)
const todos = ref([])
const newTodo = ref('')
const showTimer = ref(true)

const addTodo = () => {
    todos.value.push({
        title: newTodo.value,
        completed: false,
        date: Date.now()
    })
    newTodo.value = ''
}

const sortedTodo = computed(() => {
    console.log("demo")
    const sortedTodo = todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)
    if (hideCompleted.value == true) {
        return sortedTodo.filter(t => t.completed == false)
    }
    return sortedTodo
})

const remainingTodos = computed(() => {
    return todos.value.filter(t => t.completed == false).length
})

/*

Avant correction
const taches = ref([])
const newtache = ref('')

taches.value = [{
    title: 'Tâche à faire',
    completed: false,
    date: Date.now()
}]
const addtache = () => {
    taches.value.push({
        title: newtache.value,
        completed: false,
        date: Date.now()
    })
}
const completed = (tache) => {
    taches.value.filter(m => m === tache).completed = true

}
*/
</script>

<style>
.completed {
    opacity: .5;
    text-decoration: line-through;
}
</style>