<script setup>
const route = useRoute()
const todoId = route.params.id

const BASE_URL = "https://655eb283879575426b43d0a4.mockapi.io"

const {data: todo} = await useFetch(`${BASE_URL}/todos/${todoId}`)
  
</script>

<template>
    <div>
        {{ todoId }}
        {{ todo.name }}
    </div>
</template>