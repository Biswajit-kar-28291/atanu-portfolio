<template>
  <div class="portfolio-section" ref="sectionRef">

    <h1 class="title">My Featured Projects</h1>

    <div class="stage" ref="stageRef">

      <div
        v-for="(card, i) in cards"
        :key="card.id"
        class="card"
        :class="{ active: i === activeIndex }"
        :style="getCardStyle(i)"
        @click="activeIndex = i"
      >

        <img :src="card.image" class="card-image" />

        <div class="overlay"></div>

        <div class="content">
          <span class="tag">{{ card.category }}</span>

          <h2>{{ card.title }}</h2>

          <p>{{ card.description }}</p>

          <button>View Project →</button>
        </div>

      </div>

    </div>

    <!-- CONTROLS -->
    <div class="controls">
      <button @click="prev">←</button>

      <div class="dots">
        <span
          v-for="(card,i) in cards"
          :key="i"
          class="dot"
          :class="{active:i===activeIndex}"
          @click="activeIndex=i"
        />
      </div>

      <button @click="next">→</button>
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const activeIndex = ref(2);
const sectionRef = ref(null);
const stageRef = ref(null);

let autoSlide = null;

/* DATA */
const cards = [
  {
    id: 1,
    category: "Web Design",
    title: "Creative Agency",
    description: "Modern agency website with animations.",
    image: "https://images.unsplash.com/photo-1498050108023-c5249f4df085?w=800"
  },
  {
    id: 2,
    category: "Mobile App",
    title: "Fitness Tracker",
    description: "Beautiful UI for mobile application.",
    image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?w=800"
  },
  {
    id: 3,
    category: "Portfolio",
    title: "Developer Portfolio",
    description: "Interactive personal portfolio website.",
    image: "https://images.unsplash.com/photo-1507238691740-187a5b1d37b8?w=800"
  },
  {
    id: 4,
    category: "Photography",
    title: "Creative Studio",
    description: "Photography showcase.",
    image: "https://images.unsplash.com/photo-1492691527719-9d1e07e534b4?w=800"
  },
  {
    id: 5,
    category: "Digital Art",
    title: "3D Design",
    description: "Immersive visual experiences.",
    image: "https://images.unsplash.com/photo-1513364776144-60967b0f800f?w=800"
  }
];

/* DESKTOP 3D */
function getCardStyle(i) {
  const isMobile = window.innerWidth <= 768;

  if (isMobile) return {};

  const diff = i - activeIndex.value;
  const abs = Math.abs(diff);

  return {
    transform: `
      translateX(${diff * 180}px)
      translateZ(${-abs * 80}px)
      rotateY(${diff * -12}deg)
      scale(${i === activeIndex.value ? 1.1 : 0.85})
    `,
    opacity: abs > 2 ? 0 : 1,
    zIndex: 10 - abs
  };
}

/* NAVIGATION */
function next() {
  activeIndex.value = (activeIndex.value + 1) % cards.length;
  scrollToActive();
}

function prev() {
  activeIndex.value =
    (activeIndex.value - 1 + cards.length) % cards.length;
  scrollToActive();
}

/* MOBILE SCROLL */
function scrollToActive() {
  if (!stageRef.value) return;

  const el = stageRef.value.children[activeIndex.value];

  if (el) {
    el.scrollIntoView({
      behavior: "smooth",
      inline: "center"
    });
  }
}

/* AUTO SCROLL ONLY WHEN SECTION VISIBLE */
function observeSection() {
  const observer = new IntersectionObserver(
    (entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          startAuto();
        } else {
          stopAuto();
        }
      });
    },
    { threshold: 0.4 }
  );

  if (sectionRef.value) {
    observer.observe(sectionRef.value);
  }
}

function startAuto() {
  if (autoSlide) return;

  autoSlide = setInterval(() => {
    next();
  }, 2500);
}

function stopAuto() {
  clearInterval(autoSlide);
  autoSlide = null;
}

onMounted(() => {
  observeSection();
});

onUnmounted(() => {
  stopAuto();
});
</script>

<style scoped>
/* MAIN */
.portfolio-section {
  min-height: 100vh;
  background: #090909;
  color: white;

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  padding: 40px 20px;
  overflow: hidden;
}

.title {
  font-size: 3.5rem;
  margin-bottom: 60px;
}

/* STAGE */
.stage {
  position: relative;
  width: 100%;
  max-width: 1200px;
  height: 500px;

  display: flex;
  justify-content: center;

  perspective: 1500px;
  z-index: 1;
}

/* CARD */
.card {
  position: absolute;

  width: 320px;
  height: 450px;

  border-radius: 30px;
  overflow: hidden;

  cursor: pointer;

  transform-style: preserve-3d;

  transition: all 0.8s cubic-bezier(.22,1,.36,1);

  z-index: 2;
}

/* IMAGE */
.card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;

  filter: blur(3px);
  transform: scale(1.1);
}

.card.active .card-image {
  filter: blur(1px);
}

/* OVERLAY */
.overlay {
  position: absolute;
  inset: 0;
  pointer-events: none;

  background: linear-gradient(
    to top,
    rgba(0,0,0,0.95),
    rgba(0,0,0,0.6),
    rgba(0,0,0,0.2)
  );
}

/* CONTENT */
.content {
  position: absolute;
  bottom: 25px;
  left: 20px;
  right: 20px;

  z-index: 3;
  pointer-events: auto;
}

.tag {
  padding: 6px 14px;
  border-radius: 20px;

  background: rgba(255,255,255,.15);
  backdrop-filter: blur(10px);

  font-size: 12px;
  color: rgba(255,255,255,.9);
}

.content h2 {
  margin: 12px 0;
  font-size: 26px;

  color: #fff;
  text-shadow: 0 3px 15px rgba(0,0,0,.7);
}

.content p {
  font-size: 14px;
  color: rgba(255,255,255,.9);
}

button {
  margin-top: 12px;
  padding: 10px 18px;

  border: none;
  border-radius: 25px;

  background: rgba(255,255,255,.15);
  color: white;

  cursor: pointer;

  position: relative;
  z-index: 10;
}

/* CONTROLS */
.controls {
  margin-top: 40px;
  display: flex;
  gap: 20px;
  align-items: center;

  position: relative;
  z-index: 100;
}

.dots {
  display: flex;
  gap: 8px;
}

.dot {
  width: 8px;
  height: 8px;
  background: gray;
  border-radius: 50%;
}

.dot.active {
  width: 20px;
  background: white;
}

/* MOBILE */
@media (max-width: 768px) {

  .title {
    font-size: 2rem;
  }

  .stage {
    display: flex;
    justify-content: flex-start;

    overflow-x: auto;
    overflow-y: hidden;

    gap: 16px;
    padding: 20px;

    height: auto;

    scroll-snap-type: x mandatory;
    -webkit-overflow-scrolling: touch;

    perspective: none;
  }

  .stage::-webkit-scrollbar {
    display: none;
  }

  .card {
    position: relative !important;

    flex: 0 0 auto;

    width: 260px;
    height: 360px;

    transform: none !important;
    opacity: 1 !important;

    scroll-snap-align: center;
  }

  .card.active {
    transform: scale(1.05) !important;
  }
}
</style>