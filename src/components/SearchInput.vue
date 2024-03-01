<script setup>
import { reactive } from "vue";
const emit = defineEmits(["place-data"]); // emitir el evento place-data para enviar los datos del lugar seleccionado al componente padre (App.vue)

const searchTerm = reactive({
  // esta variable es reactiva y se puede usar en el template sin necesidad de usar el return en el setup
  value: "", // valor inicial del input de busqueda
  //suggestions: [], // sugerencias de busqueda
  timeout: null, // tiempo de espera para realizar la busqueda
  results: null,
});

const handleSearch = () => {
  clearTimeout(searchTerm.timeout); // limpiar el tiempo de espera
  searchTerm.timeout = setTimeout(async () => {
    // esperar 500ms para realizar la busqueda
    //console.log(searchTerm.value); // mostrar el valor del input de busqueda
    if (searchTerm.value !== "") {
      const resp = await fetch(
        `https://api.weatherapi.com/v1/search.json?key=ee53ff2c52524fd5a5712202240103&q=${searchTerm.value}`
      );
      const data = await resp.json(); // convertir la respuesta a json
      searchTerm.results = data; // guardar la respuesta en la variable results
      // console.log(searchTerm.results); // mostrar la respuesta en consola
      //console.log(data); // mostrar la respuesta en consola
    } else {
      searchTerm.results = null; // si el input de busqueda esta vacio, no mostrar nada
    }
  }, 500); // tiempo de espera
};


const getWeather = async(id) => {
  //console.log(id);
  const res = await fetch(`https://api.weatherapi.com/v1/forecast.json?key=ee53ff2c52524fd5a5712202240103&q=id:${id}&days=3&aqi=no&alerts=no`);
  const data = await res.json();
  emit("place-data", data);
};
</script>

<template>
  <div>
    <!--  {{ searchTerm.value }} -->
    <!-- muestra el valor del input de busqueda -->

    <form>
      <div
        class="bg-white border border-indigo-600/30 rounded-lg shadow-lg flex items-center"
      >
        <i class="fa-solid fa-magnifying-glass p-2 text-indigo-600"></i>
        <input
          type="text"
          placeholder="Search for a place"
          class="rounded-r-lg p-2 border-0 outline-0 focus:ring-2 focus:ring-indigo-600 ring-inset w-full"
          v-model="searchTerm.value"
          @input="handleSearch"
        />
        <!-- v-model para enlazar el valor del input con la variable searchTerm.value -->
        <!-- 
          @input="handleSearch" 
          evento que se dispara cada vez que se escribe en el input de busqueda 
        -->
      </div>
    </form>
    <!-- search suggestions -->
    <div class="bg-white my-2 rounded-lg shadow-lg">
      <div v-if="searchTerm.results !== null">
        <!-- place significa lugar -->
        <div v-for="place in searchTerm.results" :key="place.id">
          <button
            @click="getWeather(place.id)"
            class="px-3 my-2 hover:text-indigo-600 hover:font-bold w-full text-left"
          >
          {{ place.name }}, {{ place.region }}, {{ place.country }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
