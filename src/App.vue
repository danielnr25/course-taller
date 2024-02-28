<script setup>
import { ref } from "vue";
import SearchInput from "./components/SearchInput.vue";
import WeatherCard from "./components/WeatherCard.vue";

const places = ref([]); // array de lugares seleccionados por el usuario

const addPlace = (data) => {
  // funcion para agregar un lugar al array de places
  console.log(data); // mostrar el lugar seleccionado en consola
  places.value.push(data); // agregar el lugar al array de places
};
</script>

<template>
  <main class="mx-auto max-w-7xl px-8">
    <!-- CONFIGURACION DATE -->
    <div class="text-blue-800 font-semibold text-lg capitalize" >
      {{
        new Date().toLocaleDateString("es-ES", {
          weekday: "long",
          year: "numeric",
          month: "long",
          day: "numeric",
        })
      }}
    </div>

    <div class="pt-3 pb-8">
      <SearchInput @place-data="addPlace" />
    </div>
   
    <div class="grid lg:grid-cols-3 gap-4 sm:grid-cols-1">
      <div v-for="(place,idx) in places" :key="idx">
        <WeatherCard :place="place" class="max-w-md mx-auto" />
      </div>
    </div>
  </main>
</template>
