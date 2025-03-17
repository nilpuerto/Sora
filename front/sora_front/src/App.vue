<script setup>
import { RouterLink, RouterView } from 'vue-router'
import NavBar from './views/NavBar.vue'

</script>

<template>
  
  <div id="app">   
    <img src="/sora.svg" alt="Sora Logo" id="sora-logo" />
    <NavBar />
    <RouterView />
  </div>
</template>

<style scoped>
#app {
  position: relative;
}

#sora-logo {
  position: absolute;
  top: 30px;
  left: 35px;
  width: 34px;
  height: auto;
}

@media (max-width: 768px) {
  #sora-logo {
    width: 25px;
    left: 10px;
    top:20px,
  }
}

@media (max-width: 480px) {
  #sora-logo {
    width: 25px;
    top: 20px;
    left: 23px;
  }
}
</style>