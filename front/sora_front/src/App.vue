<script setup>
import { RouterLink, RouterView } from 'vue-router'
import NavBar from './views/NavBar.vue'


</script>

<template>
  
  <div id="app">   
    <Particles />
    <NavBar />
    <RouterView />
  </div>
</template>

<style scoped>
#app {
  position: relative;
}

::-webkit-scrollbar {
  width: 8px; /* Cambia el ancho de la barra de desplazamiento */
}

::-webkit-scrollbar-track {
  background: transparent; /* Fondo del track de la barra */
}

::-webkit-scrollbar-thumb {
  background: black; /* Color de la barra de desplazamiento */
  border-radius: 10px; /* Bordes redondeados */
}

::-webkit-scrollbar-thumb:hover {
  background: #333; /* Color más claro al pasar el mouse */
}

</style>