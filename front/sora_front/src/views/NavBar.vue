<template>
  <nav class="navbar">
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
import { RouterLink } from 'vue-router'
</script>

<style scoped>
.navbar {
  position: fixed;
  top: 22px;
  left: 60%;
  transform: translateX(-50%);
  width: 75%;
  padding: 15px 20px;
  display: flex;
  align-items: center;
  z-index: 1000;
  
}

.nav-links {
  list-style: none;
  display: flex;
  gap: 40px; 
  margin: 0 auto; 
  padding: 0;
}

.nav-links li {
  font-size: 18px;
  
}

.nav-links a {
  text-decoration: none;
  color: rgb(255, 255, 255);
  transition: color 0.3s ease;
  font-size: 15px;
}

.nav-links a:hover {
  color: #e1fd12; 
}

#sign-in-button {
  display: inline-block;
  padding: 5px 10px;
  font-size: 17px;
  font-weight: bold;
  color: rgb(0, 0, 0);
  background-color: #e1fd12;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  transition: background-position 0.5s ease, box-shadow 0.5s ease;
  background-size: 200% 200%;
  background-image: linear-gradient(135deg, #e1fd12 50%, #d4e10c 50%);
  background-position: 0 0;
  width: 100px;
  margin-left: auto; 
}

#sign-in-button:hover {
  background-position: 100% 100%;
  box-shadow: 0 0 10px #e1fd12;
}
</style>