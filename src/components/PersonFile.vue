<template>
    <div class="bg-red-400 p-4 flex flex-col">
        <span>{{person}}</span>
        <span>{{person.surname}}</span>
        <span>{{person.age}}</span>

    </div>
</template>

<script setup lang="ts">
defineProps({
  person: Object,
})
</script>
