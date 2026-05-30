<script setup>
import { ref, onMounted, onUnmounted } from "vue";

const showNavbar = ref(true);

let lastScroll = 0;

const handleScroll = () => {
  const currentScroll = window.scrollY;

  // show immediately when scrolling up
  if (currentScroll < lastScroll) {
    showNavbar.value = true;
  }

  // hide when scrolling down
  else {
    showNavbar.value = false;
  }

  // always show at top
  if (currentScroll <= 10) {
    showNavbar.value = true;
  }

  lastScroll = currentScroll;
};
onMounted(() => {
  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>
<template>
  <nav
    class="navbar navbar-expand-lg bg-transparent"
    :class="{ 'nav-hide': !showNavbar }"
  >
    <div class="container-fluid">
      <a class="navbar-brand" href="#" style="color: lightgrey">Navbar</a>

      <!-- this button when i am in my mobile -->
      <button
        class="navbar-toggler d-lg-none"
        type="button"
        data-bs-toggle="offcanvas"
        data-bs-target="#mobileMenu"
      >
        <div class="custom-toggler">
          <span></span>
          <span></span>
          <span></span>
        </div>
      </button>
      <div
        class="collapse navbar-collapse d-none d-lg-flex"
        id="navbarSupportedContent"
      >
        <ul class="navbar-nav ms-auto mb-2 mb-lg-0 me-5 gap-5">
          <li class="nav-item">
            <a class="nav-link nav-animate" aria-current="page" href="#"
              >Home</a
            >
          </li>
          <li class="nav-item">
            <a class="nav-link nav-animate" href="#">About</a>
          </li>
          <li class="nav-item">
            <a class="nav-link nav-animate" href="#">Project</a>
          </li>
          <li class="nav-item">
            <a class="nav-link nav-animate" href="#">Contact</a>
          </li>
        </ul>
      </div>

      <!-- offcanvas code -->
      <div
        class="offcanvas offcanvas-start d-lg-none"
        tabindex="-1"
        id="mobileMenu"
      >
        <div class="offcanvas-header">
          <h5 class="offcanvas-title">Menu</h5>

          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="offcanvas"
          ></button>
        </div>

        <div class="offcanvas-body">
          <ul class="navbar-nav mx-auto mb-2 mb-lg-0 gap-3">
            <li class="nav-item">
              <a class="nav-link" aria-current="page" href="#">Home</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">About</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Project</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="#">Contact</a>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </nav>
  <div>
    <!-- <img
      src="data:image/jpeg;base64,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"
      alt="dghd"
      width="100%"
    /> -->
  </div>
</template>

<style scoped>
.nav-animate {
  position: relative;
  color: lightgrey;
  transition: color 0.3s ease;
}

.nav-animate::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;

  width: 0%;
  height: 2px;

  background-color: aliceblue;
  transition: width 0.3s ease;
}

.nav-animate:hover::after {
  width: 100%;
}

.nav-animate:hover {
  color: aliceblue;
  transform: scale(1.1);
}

/* Navbar animation */
.nav-item {
  opacity: 0;
  transform: translateY(-20px);

  animation: navAnimation 0.6s ease forwards;
}

/* Delay for each item */
.nav-item:nth-child(1) {
  animation-delay: 0.2s;
}

.nav-item:nth-child(2) {
  animation-delay: 0.4s;
}

.nav-item:nth-child(3) {
  animation-delay: 0.6s;
}

.nav-item:nth-child(4) {
  animation-delay: 0.8s;
}

/* Keyframes */
@keyframes navAnimation {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
/* Remove default button style */
.navbar-toggler {
  border: none !important;

  box-shadow: none !important;
  background: transparent !important;
  padding: 0;
}
.navbar-toggler:focus {
  box-shadow: none !important;
}
/* Hamburger */
.custom-toggler {
  width: 22px;
  height: 16px;
  animation: menuAppear 0.8s ease;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

/* Lines */
.custom-toggler span {
  display: block;

  width: 100%;
  height: 3px;

  background: lightgrey;

  border-radius: 10px;

  transition: all 0.4s ease;
}
/* Desktop hover */
.navbar-toggler:hover span:nth-child(1) {
  transform: translateX(6px);
}

.navbar-toggler:hover span:nth-child(2) {
  transform: translateX(-6px);
}

.navbar-toggler:hover span:nth-child(3) {
  transform: translateX(6px);
}

/* Mobile tap animation */
.navbar-toggler:active span:nth-child(1) {
  transform: rotate(45deg) translateY(8px);
}

.navbar-toggler:active span:nth-child(2) {
  opacity: 0;
}

.navbar-toggler:active span:nth-child(3) {
  transform: rotate(-45deg) translateY(-8px);
}
@keyframes menuAppear {
  0% {
    opacity: 0;
    transform: translateY(-20px) scale(0.5);
  }

  100% {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}
.navbar {
  position: fixed;
  top: 0;
  left: 0;

  width: 100%;
  z-index: 1000;

  transition: transform 0.4s ease;
}

/* Hide navbar */
.nav-hide {
  transform: translateY(-100%);
}
</style>
