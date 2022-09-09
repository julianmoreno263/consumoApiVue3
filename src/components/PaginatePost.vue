<template>
    <div class="btn-group" role="group" aria-label="Basic example">
        <button @click="$emit('prev')" type="button" class="btn btn-outline-primary" :disabled="inicio===0">Previus
            {{inicio}}</button>
        <button @click="$emit('next')" type="button" class="btn btn-outline-primary" :disabled="fin===100">Next
            {{fin}}</button>
    </div>
</template>

<script setup>
defineProps(["inicio", "fin"])

</script>

<style scoped>

</style>