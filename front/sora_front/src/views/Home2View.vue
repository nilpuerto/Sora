<template>
  <section id="home2-section">
    <div class="line"></div>
    <h1 class="title">
  <span class="icon-circle">*</span>
    What you'll get
</h1>
<h2 class="subtitle">We resolve problems associated with <br> creative procedures.</h2>
    <div class="benefits-container">
      <div class="benefit" @mouseenter="startGauge" @mouseleave="resetGauge">
        <div class="benefit-background"></div>
        <h2>100% Voice Command</h2>
        <p>Simply speak, and Sora instantly understands and acts on your commands, providing a hands-free experience.</p>
        <div class="gauge-container">
          <div class="gauge">
            <div class="gauge-fill" :style="gaugeStyle"></div>
            <div class="gauge-cover">{{ gaugePercentage }}%</div>
          </div>
          <div class="gauge-numbers">
            <span>0</span>
            <span>20</span>
            <span>40</span>
            <span>60</span>
            <span>80</span>
            <span>100</span>
          </div>
        </div>
      </div>
      <div class="benefit" @mouseenter="flipNotifications" @mouseleave="resetNotifications">
  <div class="benefit-background"></div>
  <h2>Real-Time Notifications</h2>
  <p>Never miss an important moment. Sora keeps you updated with real-time notifications.</p>
  <div class="benefit-buttons">
    </div>
  <div class="notification-container">
    <div class="notification-card" :class="{ flipped: isFlipped }">
    </div>
    <div class="notification-card notification-back" :class="{ flipped: isFlipped }">
    </div>
  </div>
</div>
      <div class="benefit">
        <div class="benefit-background"></div>
        <h2>Privacy First</h2>
        <p>Your data is secure. Sora prioritizes your privacy and ensures your data is protected.</p>
        <div class="benefit-buttons">
    </div>
      </div>
    </div>

    <div class="footer-text">
         <span class="left-text">•&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(A futuristic voice-powered AI)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;•<br>built to organize your world</span>
        <span class="right-text">ⓒ2025</span>
    </div>
      <div class="footer-image">
      <img src="/photo1.jpg" alt="Footer Image" />
     <div class="footer-buttons"><button class="hashtag">SORA WEBSITE ↗</button>
        <button class="hashtag-button1">IOS ↗</button>
        <button class="hashtag-button1">ANDROID ↗</button></div>
      
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted, inject } from 'vue';

const particlesEnabled = ref(true);

onMounted(() => {
  if (particlesEnabled.value) {
    const container = document.createElement('div');
    container.className = 'particles-container';
    document.body.appendChild(container);

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
  }
});

onUnmounted(() => {
  const container = document.querySelector('.particles-container');
  if (container) {
    container.remove();
  }
});
const gaugePercentage = ref(0);
const gaugeStyle = ref({});
const isFlipped = ref(false);
let audioContext, analyser, microphone, dataArray;

const startAudioProcessing = async () => {
  try {
    const stream = await navigator.mediaDevices.getUserMedia({ audio: true });
    audioContext = new (window.AudioContext || window.webkitAudioContext)();
    analyser = audioContext.createAnalyser();
    microphone = audioContext.createMediaStreamSource(stream);
    microphone.connect(analyser);

    analyser.fftSize = 256;
    const bufferLength = analyser.frequencyBinCount;
    dataArray = new Uint8Array(bufferLength);

    const processAudio = () => {
      analyser.getByteFrequencyData(dataArray);
      const volume = Math.max(...dataArray); 
      const percentage = Math.round(Math.min((volume / 256) * 100, 100)); 
      gaugePercentage.value = percentage;
      gaugeStyle.value = {
        width: `${percentage}%`,
        boxShadow: `0 0 10px #e1fd12, 0 0 ${percentage / 2}px #e1fd12`,
      };
      requestAnimationFrame(processAudio); 
    };

    processAudio();
  } catch (err) {
    console.error('Error al acceder al micrófono:', err);
  }
};


// Inicialización
onMounted(() => {
  startAudioProcessing();
  const container = document.getElementById('planet-container');
  const cleanupPlanet = initPlanet(container);
  
  onUnmounted(() => {
    stopAudioProcessing();
    cleanupPlanet();
  });
});

const stopAudioProcessing = () => {
  if (audioContext) audioContext.close();
};

// Funciones para las notificaciones
const flipNotifications = () => {
  isFlipped.value = true;
};
const resetNotifications = () => {
  isFlipped.value = false;
};
</script>

<style scoped>

#home2-section {
  padding: 20px;
  height: 190vh;
}

.line {
  background-color: rgb(58, 58, 58);
  width: 99%;
  height: 1px;
  margin-top: 5%;
  animation: fadeIn 1s ease-in-out; 
}


.footer-image {
  display: flex; /* Activa Flexbox */
  align-items: center; /* Centra verticalmente los elementos */
  justify-content: flex-start; /* Alinea los elementos al inicio */
  margin-top: 20px;
}

.footer-image img {
  width: 30%;
  height: 490px;
  border-radius: 10px;
  object-fit: cover;
  margin-left: 13%;
  filter: contrast(1.2) brightness(0.9); /* Ajusta el contraste y brillo */
  mix-blend-mode: multiply;
}

.footer-buttons {
  gap: 30px; /* Espaciado entre los botones */
  margin-left: 220px; /* Mueve los botones más a la derecha */
}

.hashtag {
  display: inline-block;
  padding: 8px 40px; /* Espaciado interno */
  font-size: 14px; /* Tamaño del texto */
  font-weight: bold;
  color: rgb(0, 0, 0);
  background-color: #e1fd12; /* Fondo amarillo */
  border: none;
  border-radius: 20px;
  cursor: pointer;
  transition: background-position 0.5s ease, box-shadow 0.5s ease;
  background-size: 200% 200%;
  background-image: linear-gradient(135deg, #e1fd12 50%, #d4e10c 50%);
  background-position: 0 0;
  width: 200px; /* Aumenta el ancho del botón */
  margin-right: 30px;
  white-space: nowrap; /* Evita que el texto se divida en varias líneas */
}

.hashtag:hover {
  background-position: 100% 100%;
  box-shadow: 0 0 10px #e1ff00;
}

.hashtag-button1 {
  display: inline-block;
  padding: 8px 30px; /* Reduce el tamaño del botón */
  font-size: 14px; /* Reduce el tamaño del texto */
  font-weight: bold;
  color: #e1fd12; /* Texto amarillo */
  background-color: transparent; /* Fondo transparente */
  border: 1px solid #e1fd12; /* Borde amarillo */
  border-radius: 20px;
  cursor: pointer;
  transition: background-color 0.5s ease, box-shadow 0.5s ease;
  width: 150px; /* Ajusta el ancho */
  margin-right: 30px;
}

.hashtag-button1:hover {
  background-color: #e1fd12; /* Fondo amarillo al pasar el mouse */
  color: black; /* Texto negro al pasar el mouse */
  box-shadow: 0 0 10px #e1fd12;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
.footer-text {
  margin-top: 6%;
  font-size: 500%;
  color: #b6b6b6;
  font-weight:500;
  margin-bottom: 5%;
}

.right-text{

  font-weight: 400;
  margin-left: 24%;

}

.title {
  display: flex;
  margin-top: 20px;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  color: #e1fd12;
  font-weight: 300;
  margin-top: 100px;
}

.icon-circle {
  width: 14px;
  height: 14px;
  margin-right: 10px;
  border-radius: 50%;
  background-color: #e1fd12;
  color: black;
  font-size: 18px;
  line-height: 1.2;
  text-align: center;
  vertical-align: middle; 
  position: relative;
  top: 1px; 
  font-weight: 600;
  margin-bottom: 2px; 
}


.subtitle {
  text-align: center;
  font-size: 38px;
  font-weight: 300;
  margin: 10px 0 0; 
  background: linear-gradient(to right, #ffffff 45%, #000000); 
  -webkit-background-clip: text; 
  -webkit-text-fill-color: transparent; 
}

.benefits-container {
  display: flex;
  justify-content: space-around;
  margin-top: 5%;
}

.hashtag-button {
  background-color: #e1fd12;
  color: black;
  border: none;
  border-radius: 20px;
  padding: 3px 10px; 
  font-size: 12px; 
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
  margin: 5px;
  margin-top: 3.5%
}

.hashtag-button:hover {
  background-color: #d4e10c;
}
.benefit {
  padding: 20px; 
  border-left: 1px solid rgba(255, 255, 255, 0.5);
  border-right: 1px solid rgba(255, 255, 255, 0.5);
  width: 30%;
  text-align: center;
  color: white;
  height: 200px;
  position: relative;
  overflow: hidden;
}

.benefit::before, .benefit::after {
  content: '';
  position: absolute;
  width: 0;
  height: 1px;
  background: rgba(255, 255, 255, 0.5);
  transition: width 0.5s ease-in-out;
}

.benefit::before {
  top: 0;
  left: 0;
}

.benefit::after {
  bottom: 0;
  right: 0;
}


.benefit-background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: transparent;
  transition: background 0.5s ease-in-out;
  z-index: 0;
}

.benefit:hover .benefit-background {
  background: radial-gradient(circle at top, rgba(255, 255, 255, 0.15), transparent 70%);
}

.benefit:hover::before, .benefit:hover::after {
  width: 100%;
}


.benefit h2, .benefit p, .gauge-container {
  position: relative;
  z-index: 1; 
}

.benefit h2 {
  font-size: 20px;
  font-weight: 300;
  margin-bottom: 10px;
}

.benefit p {
  font-size: 16px;
  color: rgb(163, 163, 163);
}

.benefit-icon {
  width: 50px;
  height: 50px;
  margin-top: 10px;
}

.gauge-container {
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.gauge {
  position: relative;
  width: 80%;
  height: 20px;
  background-color: rgba(255, 255, 255, 0.1);
  border-radius: 10px;
  overflow: hidden;
}

.gauge-fill {
  height: 100%;
  width: 0;
  background-color: #e1fd12;
  transition: width 0.2s ease-in-out, box-shadow 0.2s ease-in-out; 
}

.gauge-cover {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  font-size: 14px;
  color: black;
}

.gauge-numbers {
  display: flex;
  justify-content: space-between;
  width: 80%;
  margin-top: 5px;
  font-size: 14px;
  color: white;
}

@keyframes shine {
  0% {
    box-shadow: 0 0 10px #e1fd12, 0 0 20px #e1fd12, 0 0 30px #e1fd12;
  }
  50% {
    box-shadow: 0 0 20px #ffff00, 0 0 40px #ffff00, 0 0 60px #ffff00;
  }
  100% {
    box-shadow: 0 0 10px #e1fd12, 0 0 20px #e1fd12, 0 0 30px #e1fd12;
  }
}

</style>