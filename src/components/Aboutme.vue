<template>
  <div class="page">

    <!-- ABOUT SECTION (now includes 3D background) -->
    <section ref="aboutRef" class="about" :class="{ show: showAbout }">

      <!-- 3D Background Objects -->
      <div class="scene">
        <div class="cube cube1"></div>
        <div class="cube cube2"></div>
        <div class="cube cube3"></div>
      </div>

      <!-- Content -->
      <div class="about-content">
        <h2>ABOUT ME</h2>

        <p>
          I am a passionate Computer Science student and developer focused on
          building modern UI/UX experiences, web apps, and interactive designs.
          I love turning ideas into smooth digital experiences.
        </p>

        <div class="skills">
          <span>Vue.js</span>
          <span>JavaScript</span>
          <span>Tailwind</span>
          <span>UI/UX</span>
          <span>Three.js</span>
        </div>
      </div>

    </section>

  </div>
</template>

<script setup>
import { ref, onMounted } from "vue"

const showAbout = ref(false)
const aboutRef = ref(null)

onMounted(() => {
  const observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        showAbout.value = true
      }
    },
    { threshold: 0.3 }
  )

  if (aboutRef.value) {
    observer.observe(aboutRef.value)
  }
})
</script>

<style scoped>
/* PAGE */
.page {
  background: #000;
  color: white;
  font-family: Arial, sans-serif;
  overflow-x: hidden;
}

/* ABOUT SECTION */
.about {
  min-height: 100vh;
  padding: 120px 20px;
  background: #0a0a0a;
  position: relative;
  overflow: hidden;

  opacity: 0;
  transform: translateY(80px) scale(0.9);
  transition: all 1s ease;

  display: flex;
  align-items: center;
  justify-content: center;
}

.about.show {
  opacity: 1;
  transform: translateY(0) scale(1);
}

/* CONTENT */
.about-content {
  position: relative;
  z-index: 2;
  text-align: center;
}

.about h2 {
  font-size: 50px;
  margin-bottom: 20px;
}

.about p {
  max-width: 700px;
  margin: auto;
  opacity: 0.8;
  line-height: 1.6;
}

/* SKILLS */
.skills {
  margin-top: 30px;
}

.skills span {
  display: inline-block;
  margin: 8px;
  padding: 8px 16px;
  border: 1px solid white;
  border-radius: 20px;
  font-size: 14px;
  transition: 0.3s;
}

.skills span:hover {
  background: white;
  color: black;
}

/* 3D SCENE (NOW INSIDE ABOUT) */
.scene {
  position: absolute;
  inset: 0;
  perspective: 1000px;
  overflow: hidden;
  z-index: 1;
}

.cube {
  position: absolute;
  width: 120px;
  height: 120px;
  background: rgba(255,255,255,0.05);
  border: 1px solid rgba(255,255,255,0.2);
  backdrop-filter: blur(5px);
  transform-style: preserve-3d;
  animation: float 8s infinite ease-in-out;
}

.cube1 { top: 20%; left: 10%; animation-delay: 0s; }
.cube2 { top: 60%; left: 70%; animation-delay: 2s; }
.cube3 { top: 30%; left: 80%; animation-delay: 4s; }

@keyframes float {
  0%   { transform: rotateX(0) rotateY(0) translateY(0); }
  50%  { transform: rotateX(180deg) rotateY(180deg) translateY(-40px); }
  100% { transform: rotateX(360deg) rotateY(360deg) translateY(0); }
}
</style>