<template>
  <main>
    <section id="home-section">
      <video autoplay muted loop id="background-video">
        <source src="/video_Sora.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <TheWelcome />
      <footer>
        <img src="/soralogo.svg" alt="Sora Logo" id="sora-logo"/>
        <p id="footer-text">Sora redefines how you interact with technology—just speak, and AI takes care of the rest. With advanced voice recognition and intelligent scheduling, you can create events effortlessly.

No typing, no hassle—just seamless <span class="colorFont">AI-driven</span> assistance. Welcome to the future with Sora.</p>
        <button id="get-started-button">Get Started</button>

        <font-awesome-icon :icon="['fas', 'chevron-up']" class="arrowIcon" />
      </footer>
    </section>
    <Home2View />
  </main>
</template>

<script setup>
import Home2View from './Home2View.vue';
window.addEventListener('scroll', () => {
    const elementos = document.querySelectorAll('.elemento');
    elementos.forEach(el => {
        const rect = el.getBoundingClientRect();
        if (rect.top < window.innerHeight && rect.bottom > 0) {
            el.classList.add('visible');
        } else {
            el.classList.remove('visible');
        }
    });
});
</script>

<style scoped>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
}
.elemento {
    opacity: 0;
    transition: opacity 0.5s ease-in-out;
}
.elemento.visible {
    opacity: 1;
}

main {
  position: relative;
  width: 100%;
  height: 100vh;
  overflow-x: hidden; 
  overflow-y: auto; 
  user-select: none;
}

.arrowIcon {
  position: absolute; 
  top:340px;
  right: 70px; 
  color: white;
  font-size: 24px;
  rotate: 180deg;
  cursor: pointer;
  opacity: 0;
  animation: arrowAnimationReverse 2s infinite; 

}

@keyframes arrowAnimationReverse {
  0% {
    opacity: 0;
    transform: translateY(20px); 
  }
  50% {
    opacity: 1;
    transform: translateY(10px); 
  }
  100% {
    opacity: 0;
    transform: translateY(0); 
  }
}

#home-section {
  position: relative;
  width: 100%;
  height: 100vh; 
  overflow: hidden;
}

#background-video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  z-index: -1;
  opacity: 0.7;
  mix-blend-mode: lighten; 
}

footer {
  position: relative;
  top: 55%;
  left: 1.1%;
  width: 100%;
  color: white;
}

#sora-logo {
  width: 400px;
  height: auto;
  margin-left: 20px; 
  margin-top: 20px;
  user-select: none;
  pointer-events: none;
}

#footer-text {
  font-size: 18px;
  width: 34%;
  margin-left: 20px;
  margin-bottom: 10px; 
  line-height: 1.4;
  margin-top: 0;
}

#get-started-button {
  display: inline-block;
  padding: 5px 10px;
  font-size: 17px;
  font-weight: bold ;
  color: rgb(0, 0, 0);
  background-color: #e1fd12;
  border: none;
  border-radius: 20px;
  cursor: pointer;
  transition: background-position 0.5s ease, box-shadow 0.5s ease;
  background-size: 200% 200%;
  background-image: linear-gradient(135deg, #e1fd12 50%, #d4e10c 50%);
  background-position: 0 0;
  margin-top: 20px;
  margin-left: 20px;
  width: 130px;
}

#get-started-button:hover {
  background-position: 100% 100%;
  box-shadow: 0 0 10px #e1ff00;
}

.colorFont {
  color: #e1fd12;
}

@media (max-width: 768px) {
  #background-video { 
    width: 120%;
    height: 120%;
  }

  #sora-logo {
    width: 40px;
    margin-left: 10px; 
  }
}

@media (max-width: 480px) {
  #background-video {
    left: 65%; 
    width: 140%;
    height: 140%;
  }

  #sora-logo {
    width: 30px;
    margin-left: 5px; 
  }
}
</style>