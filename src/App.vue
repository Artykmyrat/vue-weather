<script setup>
import axios from 'axios'
import { ref } from 'vue'

const location = ref('')
const temp = ref('')
const desc = ref('')
const searchQuery = ref('')
const resetQuery = () => {
  searchQuery.value = ''
}

const getWeather = async () => {
  try {
    axios
      .get(
        `http://api.weatherapi.com/v1/current.json?key=44f8a40fe9be4ff1a6890611250201&q=${searchQuery.value}&aqi=no`,
      )
      .then((res) => {
        location.value = res.data.location.name
        temp.value = res.data.current.temp_c
        desc.value = res.data.current.condition.text
      })
    searchQuery()
  } catch (error) {
    console.log(error)
    searchQuery()
  }
}
</script>

<template>
  <div class="h-screen w-screen">
    <div
      class="max-w-[900px] w-full mx-auto z-20 px-4 gap-5 flex items-center justify-center h-screen flex-col"
    >
      <div class="bg-white p-2 rounded-lg w-full gap-4 flex">
        <input
          type="text"
          class="outline-0 bg-white/60 placeholder:text-black text-3xl border border-cyan-500 w-11/12 rounded-lg py-1 px-6"
          placeholder="Search"
          v-model="searchQuery"
        />
        <button
          class="outline-0 bg-cyan-400 font-semibold rounded-lg py-1 px-6 text-3xl"
          @click="getWeather"
        >
          Search
        </button>
      </div>

      <div class="overflow-hidden rounded-lg w-full flex gap-2">
        <p class="bg-white flex-1 p-3 text-4xl rounded-md">{{ location }}</p>
        <p class="bg-white p-3 text-4xl rounded-md">{{ temp }}°С</p>
        <p class="bg-white p-3 text-4xl rounded-md">{{ desc }}</p>
      </div>
    </div>

    <img
      src="https://cdn1.ozone.ru/s3/multimedia-z/6613354475.jpg"
      alt="Loading..."
      class="h-screen w-screen absolute top-0 -z-10 object-cover"
    />
  </div>
</template>

<style scoped></style>
