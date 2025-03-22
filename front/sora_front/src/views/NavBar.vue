<template>
  <nav :class="['navbar', { 'scrolled': isScrolled }]">
    <img src="/sora.svg" alt="Sora Logo" id="sora-logo" />
    <ul class="nav-links">
      <li><RouterLink to="/">Home</RouterLink></li>
      <li><RouterLink to="/about">About</RouterLink></li>
      <li><RouterLink to="/sora">Sora</RouterLink></li>
      <li><RouterLink to="/contact">Contact</RouterLink></li>
      <li><RouterLink to="/futures">Futures</RouterLink></li>
    </ul>
    <button id="sign-in-button">Sign In</button>
  </nav>
</template>

<script setup>
import { RouterLink } from 'vue-router';
import { ref, onMounted, onUnmounted } from 'vue';

const isScrolled = ref(false);

const handleScroll = () => {
  isScrolled.value = window.scrollY > 10;
};

onMounted(() => {
  window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
});
</script>

<style scoped>
.navbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: fixed;
  top: 3px;
  left: 0;
  width: 98.2%;
  padding: 20px 1.5% 20px 1.2%;
  z-index: 1000; 
}

.navbar.scrolled {
  background: rgba(0, 0, 0, 0.8); 
  backdrop-filter: blur(20px); 
  -webkit-backdrop-filter: blur(20px); 
  border-bottom: 1px solid transparent;
  border-image: linear-gradient(to bottom, rgba(255, 255, 255, 0.1), rgba(255, 255, 255, 0));
  border-image-slice: 1;
}

#sora-logo {
  width: 30px; 
  margin-left: 10px; 
}

.nav-links {
  display: flex;
  gap: 38px; 
  list-style: none;
  margin: 0;
  padding: 0;
}

.nav-links li {
  font-size: 14px; 
}

.nav-links a {
  text-decoration: none;
  color: white;
  transition: color 0.3s ease;
}

.nav-links a:hover {
  color: #e1fd12; 
}

#sign-in-button {
  display: inline-block;
  padding: 5px 10px;
  font-size: 14px;
  font-weight: bold;
  color: black;
  background-color: #e1fd12;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  transition: background-position 0.5s ease, box-shadow 0.5s ease; 
  background-size: 200% 200%; 
  background-image: linear-gradient(135deg, #e1fd12 50%, #d4e10c 50%); 
  background-position: 0 0;
  margin-right: 20px;
}

#sign-in-button:hover {
  background-position: 100% 100%; 
  box-shadow: 0 0 10px #e1fd12; 
}
</style>