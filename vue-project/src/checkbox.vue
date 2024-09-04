<template>
    <label>
        <input type="checkbox" v-model="model">
        {{ label }}
    </label>
</template>

<script setup>
const model = defineModel()
// permet de descendre l'information
const props = defineProps({
    label: String
})
console.log(props.label)

// permet de monter l'information 
const emits = defineEmits(['check', 'uncheck'])

const onChange = (event) => {
    if (event.currentTarget.checked) {
        emits('check', event.currentTarget)
    } else {
        emits('uncheck', event.currentTarget)
    }
}
</script>