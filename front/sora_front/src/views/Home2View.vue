<template>
  <section id="home2-section">
    <div class="line"></div>
    <h1 class="title">AI Benefits</h1>
    <div class="benefits-container">
      <div class="benefit">
        <h2>100% Voice Command</h2>
        <p>Simply speak, and Sora instantly understands and acts on your commands, providing a hands-free experience.</p>
        <div class="gauge-container">
          <div class="gauge">
            <div class="gauge-fill"></div>
            <div class="gauge-cover">0%</div>
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
      <div class="benefit">
        <h2>Real-Time Notifications</h2>
        <p>Never miss an important moment. Sora keeps you updated with real-time notifications.</p>
      </div>
      <div class="benefit">
        <h2>Privacy First</h2>
        <p>Your data is secure. Sora prioritizes your privacy and ensures your data is protected.</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { onMounted } from 'vue';

onMounted(() => {
  const gaugeFill = document.querySelector('.gauge-fill');
  const gaugeCover = document.querySelector('.gauge-cover');
  let fillPercentage = 0;

  const fillGauge = () => {
    if (fillPercentage <= 100) {
      gaugeFill.style.width = `${fillPercentage}%`;
      gaugeCover.textContent = `${fillPercentage}%`;
      fillPercentage++;
      requestAnimationFrame(fillGauge);
    }
  };

  fillGauge();
});
</script>

<style scoped>
#home2-section {
  padding: 20px;
  height: 100vh; 
  transition: opacity 0.5s ease-in-out;
}

.line {
  background-color: rgb(58, 58, 58); 
  width: 98%; 
  height: 1px; 
  margin-top: 5%;
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
  background-color: transparent;
  padding: 8px;
  border-left: 1px solid rgba(255, 255, 255, 0.5);
  border-right: 1px solid rgba(255, 255, 255, 0.5);
  width: 30%;
  text-align: center;
  color: white;
  height: 180px; 
  position: relative;
  transition: all 0.3s ease-in-out;
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

.benefit:hover::before {
  width: 100%;
}

.benefit:hover::after {
  width: 100%;
}

.benefit:hover {
  background: radial-gradient(circle at top, rgba(255, 255, 255, 0.2), transparent 70%);
  animation: fadeInBackground 0.5s ease-in-out forwards;
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
  transition: width 1s ease-in-out;
  animation: shine 2s infinite;
  box-shadow: 0 0 10px #e1fd12, 0 0 20px #e1fd12, 0 0 30px #e1fd12;
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

@keyframes fadeInBackground {
  0% {
    opacity: 0.2;
  }
  100% {
    opacity: 1;
  }
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