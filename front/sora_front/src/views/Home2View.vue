<template>
  <section id="home2-section">
    <div class="line"></div>
    <h1 class="title">AI Benefits</h1>
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
      <div class="benefit" @mouseenter="showBell" @mouseleave="hideBell">
        <div class="benefit-background"></div>
        <h2>Real-Time Notifications</h2>
        <p>Never miss an important moment. Sora keeps you updated with real-time notifications.</p>
        <div class="bell-container" :class="{ active: isBellActive }">
          <i class="fas fa-bell bell-icon"></i>
        </div>
      </div>
      <div class="benefit">
        <div class="benefit-background"></div>
        <h2>Privacy First</h2>
        <p>Your data is secure. Sora prioritizes your privacy and ensures your data is protected.</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

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

const isBellActive = ref(false);

const showBell = () => {
  isBellActive.value = true;
};

const hideBell = () => {
  isBellActive.value = false;
};
</script>

<style scoped>
#home2-section {
  padding: 20px;
  height: 100vh;
}

.line {
  background-color: rgb(58, 58, 58);
  width: 98%;
  height: 1px;
  margin-top: 5%;
  animation: fadeIn 1s ease-in-out; /* Animación de entrada */
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
  margin-top: 7%;
  font-size: 34px;
  color: rgb(255, 255, 255);
  font-weight: 300;
  margin-left: 1.3%;
  padding-bottom: 5%;
}

.benefits-container {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
}

.benefit {
  padding: 8px;
  border-left: 1px solid rgba(255, 255, 255, 0.5);
  border-right: 1px solid rgba(255, 255, 255, 0.5);
  width: 30%;
  text-align: center;
  color: white;
  height: 180px;
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
  margin-top: 20px;
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

.bell-container {
  position: relative;
  margin-top: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  transform: scale(0.8);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.bell-container.active {
  opacity: 1;
  transform: scale(1);
}

.bell-icon {
  font-size: 40px;
  color: #e1fd12;
  animation: none;
}

.bell-container.active .bell-icon {
  animation: bellShake 0.5s ease infinite;
}

@keyframes bellShake {
  0% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(-15deg);
  }
  50% {
    transform: rotate(15deg);
  }
  75% {
    transform: rotate(-10deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
</style>