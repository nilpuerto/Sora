<script setup>
import { provide, ref, watch, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import NavBar from './views/NavBar.vue'; // Asegúrate de importar NavBar

const route = useRoute();
const particlesEnabled = ref(true);
const particlesOverride = ref(null);

// Proveemos el estado para componentes hijos
provide('particlesState', {
  enabled: particlesEnabled,
  override: particlesOverride
});

// Sistema de partículas
onMounted(() => {
  const container = document.querySelector('.particles-container');
  if (!container) return;
  
  const particleCount = 60;
  for (let i = 0; i < particleCount; i++) {
    const particle = document.createElement('div');
    particle.className = 'particle';
    
    particle.style.left = `${Math.random() * 100}vw`;
    particle.style.top = `${Math.random() * 100}vh`;
    particle.style.opacity = (0.1 + Math.random() * 0.3).toFixed(2);
    particle.style.width = `${1 + Math.random()}px`;
    particle.style.height = particle.style.width;
    
    particle.style.setProperty('--move-x', (Math.random() * 400 - 200));
    particle.style.setProperty('--move-y', (Math.random() * 400 - 200));
    particle.style.animationDuration = `${10 + Math.random() * 20}s`;
    
    container.appendChild(particle);
  }
});

// Control por ruta
watch(() => route.path, (path) => {
  // Solo ajusta si no hay override activo
  if (particlesOverride.value === null) {
    particlesEnabled.value = !path.startsWith('/home');
  }
}, { immediate: true });
</script>

<template>
  <div class="app-wrapper">
    <!-- Partículas de fondo -->
    <div v-show="particlesEnabled" class="particles-container"></div>
    
    <!-- NavBar visible siempre -->
    <NavBar />
    
    <!-- Contenido principal -->
    <RouterView />
  </div>
</template>

<style>
.app-wrapper {
  position: relative;
  min-height: 100vh;
}

/* Partículas */
.particles-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* Detrás de todo */
  pointer-events: none;
}

.particle {
  position: absolute;
  background-color: white;
  border-radius: 50%;
  animation: float linear infinite;
  transform: translate(-50%, -50%);
  filter: blur(0.5px);
}

@keyframes float {
  to {
    transform: translate(
      calc(-50% + var(--move-x) * 1px),
      calc(-50% + var(--move-y) * 1px)
    );
  }
}

/* Asegura que el NavBar esté por encima */
nav {
  position: relative;
  z-index: 10;
}
</style>