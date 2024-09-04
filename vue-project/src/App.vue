<template>
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
                <label>
                    <input type="checkbox" v-model="todo.completed">
                    {{ todo.title }}
                </label>

            </li>
        </ul>

        <label>
            <input type="checkbox" v-model="hideCompleted">
            Masquer les tâches complétées
        </label>
        <p v-if="remainingTodos > 0"> {{ remainingTodos }} tâche{{ remainingTodos > 1 ? 's':''}} à faire.</p>
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
import { computed, ref } from 'vue'

const hideCompleted = ref(false)
const todos = ref([{
    title: 'Tâche ',
    completed: true,
    date: Date.now()
},
{
    title: 'Tâche à faire',
    completed: false,
    date: Date.now()
}])
const newTodo = ref('')

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