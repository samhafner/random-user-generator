<script setup lang="ts">

import { Ref, ref } from 'vue'
import axios, { AxiosResponse } from 'axios'
import { RandomUser } from './interface/index'

const user = ref({} as RandomUser)

init()

async function init() {
    await getRandomUser()
    console.log(user.value)
}

async function getRandomUser() {
    try {
        const response: AxiosResponse<{ results: RandomUser[] }> = await axios.get('https://randomuser.me/api/1.4')
        user.value = response.data.results[0]
    }
    catch (error) {
        console.log(error)
    }

}

</script>

<template>
    <h1 class="text-3xl font-bold my-4">Random User</h1>
    <div class="space-y-3 flex flex-col justify-center items-center">
        <div class="rounded-full overflow-hidden h-32 w-32 border-4 border-stone-900">
            <img :src="user.picture.large" :alt="user.name.first" class="object-fit object-cover ">
        </div>
        <p class="text-xl uppercase">{{ `${user.name.first + " " + user.name.last}` }}</p>
        <p>Email: {{ user.email }}</p>
        <button @click="getRandomUser" class="px-3 py-2 bg-stone-900 rounded-lg border border-white/70 hover:bg-stone-800">Get random user</button>
    </div>

</template>

<style scoped>

</style>
