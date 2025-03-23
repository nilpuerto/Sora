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
    <div id="planet-container"></div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';

const gaugePercentage = ref(0);
const gaugeStyle = ref({});
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

const stopAudioProcessing = () => {
  if (audioContext) {
    audioContext.close();
  }
};

onMounted(() => {
  startAudioProcessing();
});

onUnmounted(() => {
  stopAudioProcessing();
});

onMounted(() => {
  const container = document.getElementById('planet-container');

  // Configuración básica
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera(75, container.offsetWidth / container.offsetHeight, 0.1, 1000);
  const renderer = new THREE.WebGLRenderer({ antialias: true });
  renderer.setSize(container.offsetWidth, container.offsetHeight);
  renderer.setClearColor(0x000000); // Fondo negro
  container.appendChild(renderer.domElement);

  // Crear la esfera (planeta)
  const geometry = new THREE.SphereGeometry(5, 64, 64); // Radio y detalle
  const textureLoader = new THREE.TextureLoader();
  const material = new THREE.MeshBasicMaterial({
    map: textureLoader.load('https://raw.githubusercontent.com/planet-texture/earth-wireframe/main/earth-wireframe.png'), // Textura del mapa mundi
    transparent: true,
  });
  const sphere = new THREE.Mesh(geometry, material);
  scene.add(sphere);

  // Configurar la cámara
  camera.position.z = 10;

  // Controles de órbita (para mover el planeta)
  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true; // Movimiento suave
  controls.dampingFactor = 0.05;
  controls.enableZoom = false; // Deshabilitar el zoom

  // Animación
  const animate = () => {
    requestAnimationFrame(animate);
    sphere.rotation.y += 0.001; // Rotación automática
    controls.update();
    renderer.render(scene, camera);
  };

  animate();

  // Ajustar el tamaño al cambiar el tamaño de la ventana
  window.addEventListener('resize', () => {
    camera.aspect = container.offsetWidth / container.offsetHeight;
    camera.updateProjectionMatrix();
    renderer.setSize(container.offsetWidth, container.offsetHeight);
  });
});


</script>

<style scoped>
#home2-section {
  padding: 20px;
  height: 140vh;
}
#planet-container {
  width: 100%;
  height: 500px; /* Ajusta la altura según lo necesario */
  margin-top: 50px;
  background-color: black; /* Fondo negro */
  overflow: hidden;
}

.line {
  background-color: rgb(58, 58, 58);
  width: 99%;
  height: 1px;
  margin-top: 5%;
  animation: fadeIn 1s ease-in-out; 
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
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