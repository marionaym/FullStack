<script setup>
defineProps(["ville"]);
</script>

<template>
    <tr>
        <td>{{ville.name}}</td>
        <td>{{ville.population}}</td>
        <td> <button class="button" @click="$emit('deleteC',ville,index)">Delete</button></td>
    </tr>
</template>