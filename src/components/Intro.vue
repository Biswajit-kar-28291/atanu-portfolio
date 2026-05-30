<script setup>
import { ref, onMounted } from 'vue'
import personImg from '@/components/stock-person-png-stock-photo-man-11563049686zqeb9zmqjd.png'

const roles = [
  'Python Developer',
  'Website Development',
  'Video Editor',
  'UI/UX Designer',
  'AI Enthusiast'
]

const displayText = ref('')
let roleIndex = 0
let charIndex = 0
let isDeleting = false

const typeEffect = () => {
  const currentRole = roles[roleIndex]

  if (!isDeleting) {
    displayText.value = currentRole.substring(0, charIndex + 1)
    charIndex++

    if (charIndex === currentRole.length) {
      isDeleting = true
      setTimeout(typeEffect, 1200)
      return
    }
  } else {
    displayText.value = currentRole.substring(0, charIndex - 1)
    charIndex--

    if (charIndex === 0) {
      isDeleting = false
      roleIndex = (roleIndex + 1) % roles.length
    }
  }

  setTimeout(typeEffect, isDeleting ? 60 : 120)
}

onMounted(() => {
  typeEffect()
})
</script>

<template>
  <section class="hero">

    <!-- Left Content -->
    <div class="hero-content">

      <h1 class="title">
        Your Name Here
      </h1>

      <h2 class="typing-text">
        I am interested in
        <span>{{ displayText }}</span>
      </h2>

      <button class="hero-btn">
        Let’s get started →
      </button>

    </div>

    <!-- Right Image -->
    <div class="hero-image">
      <img :src="personImg" />
    </div>

  </section>
</template>

<style scoped>

/* Main Section */
.hero {
  width: 100%;
  min-height: 100vh;

  background: black;
  color: white;

  display: flex;
  justify-content: space-between;
  align-items: center;

  padding-inline: clamp(20px, 5vw, 80px);

  overflow-x: hidden;
}

/* LEFT SIDE */
.hero-content {
  max-width: 550px;
  animation: leftSlide 1s ease;
}

.title {
  font-size: 70px;
  font-weight: bold;
  margin-bottom: 20px;
}

/* typing text */
.typing-text {
  font-size: 28px;
  margin-bottom: 25px;
}

.typing-text span {
  color: #00ffff;
  border-right: 3px solid #00ffff;
  padding-right: 5px;
  animation: blink 0.7s infinite;
}

@keyframes blink {
  50% {
    border-color: transparent;
  }
}

/* BUTTON */
.hero-btn {
  padding: 18px 40px;
  border: none;
  border-radius: 12px;

  background: #5b9d20;
  color: white;

  font-size: 18px;
  font-weight: bold;

  cursor: pointer;
  transition: 0.3s ease;
   box-shadow:
    0 8px 20px rgba(114, 97, 97, 0.4),
    0 0 15px rgba(106, 196, 28, 0.4);
}

.hero-btn:hover {
  transform: translateY(-4px) scale(1.05);

  box-shadow:
    0 12px 30px rgba(0, 0, 0, 0.5),
    0 0 25px rgba(91, 157, 32, 0.7);
}

.hero-btn:active  {
  box-shadow:
    0 5px 15px rgba(0, 0, 0, 0.4),
    0 0 10px rgba(91, 157, 32, 0.5);

  animation: clickPop 0.8s cubic-bezier(0.25, 1.5, 0.5, 1);
}

/* keyframes */
@keyframes clickPop {
  0% {
    transform: scale(1);
  }

  40% {
    transform: scale(0.85);
  }

  100% {
    transform: scale(1);
  }
}

/* LEFT ANIMATION */
@keyframes leftSlide {
  from {
    opacity: 0;
    transform: translateX(-150px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

/* RIGHT IMAGE CONTAINER */
.hero-image {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  animation: rightSlide 1.2s ease;
}

/* ⭐ FIXED SHADOW (VISIBLE + CLEAN BEHIND IMAGE) */
.hero-image::before {
  content: "";
  position: absolute;

  width: 440px;
  height: 440px;

  background: rgba(0, 255, 255, 0.35);

  filter: blur(100px);

  border-radius: 50%;

  /* better depth */
  transform: translateY(25px);

  z-index: 0;
}

/* IMAGE */
.hero-image img {
  width: 420px;
  height: 420px;

  object-fit: cover;
  border-radius: 50%;

  position: relative;
  z-index: 1;
}

/* RIGHT ANIMATION */
@keyframes rightSlide {
  from {
    opacity: 0;
    transform: translateX(250px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

/* MOBILE */
@media (max-width: 992px) {
  .hero {
    flex-direction: column;
    justify-content: center;
    align-items: center;

    text-align: center;

    gap: 30px;

    padding: 100px 20px 40px;

    overflow-x: hidden;
  }

  .hero-content {
    width: 100%;
    max-width: 100%;
  }

  .title {
    font-size: 45px;
    line-height: 1.2;
  }

  .typing-text {
    font-size: 24px;
  }

  .hero-btn {
    width: 100%;
    max-width: 280px;
  }

  .hero-image {
    width: 100%;
    display: flex;
    justify-content: center;
  }

  .hero-image img {
    width: 260px;
    height: 260px;

    max-width: 100%;
    display: block;
  }

  .hero-image::before {
    width: 240px;
    height: 240px;
  }
}
</style>