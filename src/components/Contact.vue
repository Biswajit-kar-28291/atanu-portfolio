<template>
  <div class="contact-page" ref="pageRef">
    <!-- 3D Canvas Background -->
    <canvas ref="canvasRef" class="bg-canvas"></canvas>

    <!-- Floating orbs -->
    <div class="orb orb-1"></div>
    <div class="orb orb-2"></div>
    <div class="orb orb-3"></div>

    <!-- Grid overlay -->
    <div class="grid-overlay"></div>

    <!-- Content -->
    <div class="content-wrapper">
      <!-- Header -->
      <div class="header" :class="{ visible: mounted }">
        <span class="label">— Let's Connect</span>
        <h1 class="title">
          <span class="title-line">Get In</span>
          <span class="title-line accent">Touch.</span>
        </h1>
        <p class="subtitle">
          Have a project in mind? I'd love to hear about it.<br />
          Drop me a message and I'll get back to you.
        </p>
      </div>

      <!-- Main Card -->
      <div class="contact-card" :class="{ visible: mounted }" @mousemove="onCardMouseMove" @mouseleave="onCardMouseLeave" ref="cardRef">
        <div class="card-inner" :style="cardTiltStyle">
          <!-- Shimmer line -->
          <div class="shimmer-line"></div>

          <!-- Form -->
          <form class="contact-form" @submit.prevent="handleSubmit">
            <div class="form-row">
              <div class="field-group" :class="{ focused: focused.name, filled: form.name }">
                <label>Name</label>
                <input
                  type="text"
                  v-model="form.name"
                  @focus="focused.name = true"
                  @blur="focused.name = false"
                  placeholder="Your full name"
                  autocomplete="off"
                />
                <div class="field-border"></div>
              </div>

              <div class="field-group" :class="{ focused: focused.email, filled: form.email }">
                <label>Email</label>
                <input
                  type="email"
                  v-model="form.email"
                  @focus="focused.email = true"
                  @blur="focused.email = false"
                  placeholder="hello@you.com"
                  autocomplete="off"
                />
                <div class="field-border"></div>
              </div>
            </div>

            <div class="field-group" :class="{ focused: focused.subject, filled: form.subject }">
              <label>Subject</label>
              <input
                type="text"
                v-model="form.subject"
                @focus="focused.subject = true"
                @blur="focused.subject = false"
                placeholder="What's this about?"
                autocomplete="off"
              />
              <div class="field-border"></div>
            </div>

            <div class="field-group textarea-group" :class="{ focused: focused.message, filled: form.message }">
              <label>Message</label>
              <textarea
                v-model="form.message"
                @focus="focused.message = true"
                @blur="focused.message = false"
                placeholder="Tell me about your project..."
                rows="5"
              ></textarea>
              <div class="field-border"></div>
            </div>

            <div class="form-footer">
              <!-- Social Links -->
              <div class="socials">
                <a v-for="s in socials" :key="s.name" :href="s.href" class="social-btn" target="_blank" rel="noopener">
                  <span class="social-icon" v-html="s.icon"></span>
                  <span class="social-name">{{ s.name }}</span>
                </a>
              </div>

              <!-- Submit Button -->
              <button class="submit-btn" type="submit" :class="{ loading: submitting, success: submitted }">
                <span v-if="!submitted" class="btn-text">
                  <span v-if="!submitting">Send Message</span>
                  <span v-else class="dots">
                    <span></span><span></span><span></span>
                  </span>
                </span>
                <span v-else class="btn-success">✓ Sent!</span>
                <div class="btn-glow"></div>
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContactPage',

  data() {
    return {
      mounted: false,
      submitting: false,
      submitted: false,
      form: { name: '', email: '', subject: '', message: '' },
      focused: { name: false, email: false, subject: false, message: false },
      cardTilt: { x: 0, y: 0 },
      animFrame: null,
      particles: [],
      socials: [
        {
          name: 'GitHub',
          href: '#',
          icon: `<svg viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.37 0 0 5.37 0 12c0 5.31 3.435 9.795 8.205 11.385.6.105.825-.255.825-.57 0-.285-.015-1.23-.015-2.235-3.015.555-3.795-.735-4.035-1.41-.135-.345-.72-1.41-1.23-1.695-.42-.225-1.02-.78-.015-.795.945-.015 1.62.87 1.845 1.23 1.08 1.815 2.805 1.305 3.495.99.105-.78.42-1.305.765-1.605-2.67-.3-5.46-1.335-5.46-5.925 0-1.305.465-2.385 1.23-3.225-.12-.3-.54-1.53.12-3.18 0 0 1.005-.315 3.3 1.23.96-.27 1.98-.405 3-.405s2.04.135 3 .405c2.295-1.56 3.3-1.23 3.3-1.23.66 1.65.24 2.88.12 3.18.765.84 1.23 1.905 1.23 3.225 0 4.605-2.805 5.625-5.475 5.925.435.375.81 1.095.81 2.22 0 1.605-.015 2.895-.015 3.3 0 .315.225.69.825.57A12.02 12.02 0 0 0 24 12c0-6.63-5.37-12-12-12z"/></svg>`
        },
        {
          name: 'Twitter',
          href: '#',
          icon: `<svg viewBox="0 0 24 24" fill="currentColor"><path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.817L4.99 21.75H1.68l7.73-8.835L1.254 2.25H8.08l4.713 6.231zm-1.161 17.52h1.833L7.084 4.126H5.117z"/></svg>`
        },
        {
          name: 'LinkedIn',
          href: '#',
          icon: `<svg viewBox="0 0 24 24" fill="currentColor"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433a2.062 2.062 0 0 1-2.063-2.065 2.064 2.064 0 1 1 2.063 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>`
        }
      ]
    }
  },

  computed: {
    cardTiltStyle() {
      return {
        transform: `perspective(1000px) rotateX(${this.cardTilt.x}deg) rotateY(${this.cardTilt.y}deg) translateZ(0)`
      }
    }
  },

  mounted() {
    setTimeout(() => { this.mounted = true }, 100)
    this.initCanvas()
    window.addEventListener('resize', this.resizeCanvas)
  },

  beforeUnmount() {
    window.removeEventListener('resize', this.resizeCanvas)
    if (this.animFrame) cancelAnimationFrame(this.animFrame)
  },

  methods: {
    initCanvas() {
      const canvas = this.$refs.canvasRef
      if (!canvas) return
      const ctx = canvas.getContext('2d')
      this.resizeCanvas()

      // Create particles
      const createParticles = () => {
        this.particles = []
        const count = Math.floor((canvas.width * canvas.height) / 12000)
        for (let i = 0; i < count; i++) {
          this.particles.push({
            x: Math.random() * canvas.width,
            y: Math.random() * canvas.height,
            z: Math.random() * 400 + 50,
            vx: (Math.random() - 0.5) * 0.3,
            vy: (Math.random() - 0.5) * 0.3,
            size: Math.random() * 1.5 + 0.5,
            opacity: Math.random() * 0.6 + 0.1,
            color: Math.random() > 0.7 ? '#00d4ff' : Math.random() > 0.5 ? '#7c3aed' : '#ffffff'
          })
        }
      }

      createParticles()

      const draw = () => {
        ctx.clearRect(0, 0, canvas.width, canvas.height)

        // Draw connections
        for (let i = 0; i < this.particles.length; i++) {
          for (let j = i + 1; j < this.particles.length; j++) {
            const p1 = this.particles[i]
            const p2 = this.particles[j]
            const dx = p1.x - p2.x
            const dy = p1.y - p2.y
            const dist = Math.sqrt(dx * dx + dy * dy)
            if (dist < 120) {
              ctx.beginPath()
              ctx.strokeStyle = `rgba(0, 212, 255, ${0.08 * (1 - dist / 120)})`
              ctx.lineWidth = 0.5
              ctx.moveTo(p1.x, p1.y)
              ctx.lineTo(p2.x, p2.y)
              ctx.stroke()
            }
          }
        }

        // Draw particles
        this.particles.forEach(p => {
          p.x += p.vx
          p.y += p.vy
          if (p.x < 0) p.x = canvas.width
          if (p.x > canvas.width) p.x = 0
          if (p.y < 0) p.y = canvas.height
          if (p.y > canvas.height) p.y = 0

          ctx.beginPath()
          ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2)
          ctx.fillStyle = p.color.replace(')', `, ${p.opacity})`).replace('rgb', 'rgba').replace('#', '')

          // Simpler fill
          ctx.globalAlpha = p.opacity
          ctx.fillStyle = p.color
          ctx.fill()
          ctx.globalAlpha = 1
        })

        this.animFrame = requestAnimationFrame(draw)
      }

      draw()
    },

    resizeCanvas() {
      const canvas = this.$refs.canvasRef
      if (!canvas) return
      canvas.width = window.innerWidth
      canvas.height = window.innerHeight
    },

    onCardMouseMove(e) {
      const card = this.$refs.cardRef
      if (!card) return
      const rect = card.getBoundingClientRect()
      const cx = rect.left + rect.width / 2
      const cy = rect.top + rect.height / 2
      const dx = (e.clientX - cx) / (rect.width / 2)
      const dy = (e.clientY - cy) / (rect.height / 2)
      this.cardTilt = { x: -dy * 4, y: dx * 4 }
    },

    onCardMouseLeave() {
      this.cardTilt = { x: 0, y: 0 }
    },

    async handleSubmit() {
      if (this.submitting || this.submitted) return
      this.submitting = true
      await new Promise(r => setTimeout(r, 1800))
      this.submitting = false
      this.submitted = true
      setTimeout(() => {
        this.submitted = false
        this.form = { name: '', email: '', subject: '', message: '' }
      }, 3000)
    }
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500&display=swap');

* { box-sizing: border-box; margin: 0; padding: 0; }

.contact-page {
  min-height: 100vh;
  width: 100%;
  background: #050810;
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'DM Sans', sans-serif;
  overflow: hidden;
  position: relative;
}

/* Canvas */
.bg-canvas {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  pointer-events: none;
}

/* Orbs */
.orb {
  position: fixed;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
  z-index: 1;
}
.orb-1 {
  width: 500px; height: 500px;
  background: radial-gradient(circle, rgba(124,58,237,0.25) 0%, transparent 70%);
  top: -100px; left: -100px;
  animation: orbFloat1 12s ease-in-out infinite;
}
.orb-2 {
  width: 400px; height: 400px;
  background: radial-gradient(circle, rgba(0,212,255,0.2) 0%, transparent 70%);
  bottom: -80px; right: -80px;
  animation: orbFloat2 15s ease-in-out infinite;
}
.orb-3 {
  width: 300px; height: 300px;
  background: radial-gradient(circle, rgba(236,72,153,0.15) 0%, transparent 70%);
  top: 50%; right: 20%;
  animation: orbFloat3 10s ease-in-out infinite;
}

@keyframes orbFloat1 {
  0%, 100% { transform: translate(0, 0) scale(1); }
  50% { transform: translate(60px, 80px) scale(1.1); }
}
@keyframes orbFloat2 {
  0%, 100% { transform: translate(0, 0) scale(1); }
  50% { transform: translate(-50px, -60px) scale(1.15); }
}
@keyframes orbFloat3 {
  0%, 100% { transform: translateY(0) scale(1); }
  50% { transform: translateY(-40px) scale(0.9); }
}

/* Grid */
.grid-overlay {
  position: fixed;
  inset: 0;
  background-image:
    linear-gradient(rgba(0,212,255,0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,212,255,0.03) 1px, transparent 1px);
  background-size: 60px 60px;
  z-index: 1;
  pointer-events: none;
}

/* Content */
.content-wrapper {
  position: relative;
  z-index: 10;
  width: 100%;
  max-width: 800px;
  padding: 40px 24px;
  display: flex;
  flex-direction: column;
  gap: 40px;
}

/* Header */
.header {
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}
.header.visible {
  opacity: 1;
  transform: translateY(0);
}

.label {
  font-family: 'DM Sans', sans-serif;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 3px;
  text-transform: uppercase;
  color: #00d4ff;
  display: block;
  margin-bottom: 12px;
}

.title {
  font-family: 'Syne', sans-serif;
  font-size: clamp(52px, 8vw, 88px);
  font-weight: 800;
  line-height: 0.95;
  letter-spacing: -2px;
  margin-bottom: 20px;
}
.title-line {
  display: block;
  color: #f0f0f8;
}
.title-line.accent {
  color: transparent;
  -webkit-text-stroke: 2px rgba(0,212,255,0.7);
}

.subtitle {
  font-size: 16px;
  line-height: 1.7;
  color: rgba(180, 185, 220, 0.7);
  max-width: 420px;
  font-weight: 300;
}

/* Card */
.contact-card {
  opacity: 0;
  transform: translateY(40px);
  transition: all 1s cubic-bezier(0.16, 1, 0.3, 1) 0.2s;
}
.contact-card.visible {
  opacity: 1;
  transform: translateY(0);
}

.card-inner {
  background: rgba(12, 16, 32, 0.8);
  border: 1px solid rgba(0, 212, 255, 0.12);
  border-radius: 24px;
  padding: 40px;
  backdrop-filter: blur(20px);
  position: relative;
  overflow: hidden;
  transition: transform 0.15s ease;
  box-shadow:
    0 0 0 1px rgba(255,255,255,0.04),
    0 40px 80px rgba(0,0,0,0.5),
    inset 0 1px 0 rgba(255,255,255,0.05);
}

/* Shimmer */
.shimmer-line {
  position: absolute;
  top: 0; left: -100%;
  width: 200%;
  height: 1px;
  background: linear-gradient(90deg, transparent, rgba(0,212,255,0.6), rgba(124,58,237,0.6), transparent);
  animation: shimmer 4s linear infinite;
}
@keyframes shimmer {
  0% { left: -100%; }
  100% { left: 100%; }
}

/* Form */
.contact-form {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 20px;
}

.field-group {
  position: relative;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.field-group label {
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 2px;
  text-transform: uppercase;
  color: rgba(150, 160, 200, 0.5);
  transition: color 0.3s;
}
.field-group.focused label { color: #00d4ff; }
.field-group.filled label { color: rgba(150, 160, 200, 0.8); }

.field-group input,
.field-group textarea {
  background: rgba(255,255,255,0.03);
  border: 1px solid rgba(255,255,255,0.06);
  border-radius: 10px;
  padding: 14px 16px;
  color: #e8eaf6;
  font-family: 'DM Sans', sans-serif;
  font-size: 15px;
  font-weight: 400;
  outline: none;
  transition: all 0.3s;
  resize: none;
}
.field-group input::placeholder,
.field-group textarea::placeholder {
  color: rgba(180,185,220,0.2);
}
.field-group.focused input,
.field-group.focused textarea {
  background: rgba(0,212,255,0.04);
  border-color: rgba(0,212,255,0.3);
  box-shadow: 0 0 20px rgba(0,212,255,0.08);
}

.field-border {
  position: absolute;
  bottom: 0; left: 0;
  width: 0; height: 2px;
  background: linear-gradient(90deg, #00d4ff, #7c3aed);
  border-radius: 0 0 10px 10px;
  transition: width 0.4s cubic-bezier(0.16,1,0.3,1);
}
.field-group.focused .field-border { width: 100%; }

/* Footer */
.form-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 16px;
  padding-top: 8px;
}

/* Socials */
.socials {
  display: flex;
  gap: 8px;
}

.social-btn {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 14px;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.07);
  border-radius: 40px;
  color: rgba(200,205,240,0.6);
  text-decoration: none;
  font-size: 12px;
  font-weight: 500;
  transition: all 0.25s;
}
.social-btn:hover {
  background: rgba(0,212,255,0.08);
  border-color: rgba(0,212,255,0.25);
  color: #00d4ff;
  transform: translateY(-2px);
}
.social-icon { width: 14px; height: 14px; display: flex; align-items: center; }
.social-icon svg { width: 100%; height: 100%; }
.social-name { display: none; }

/* Submit Button */
.submit-btn {
  position: relative;
  padding: 14px 32px;
  background: linear-gradient(135deg, #00d4ff 0%, #7c3aed 100%);
  border: none;
  border-radius: 50px;
  color: #fff;
  font-family: 'Syne', sans-serif;
  font-size: 14px;
  font-weight: 700;
  letter-spacing: 1px;
  cursor: pointer;
  overflow: hidden;
  transition: all 0.3s;
  min-width: 160px;
}
.submit-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 40px rgba(0,212,255,0.35);
}
.submit-btn:active { transform: translateY(0); }
.submit-btn.success {
  background: linear-gradient(135deg, #00e676, #00bcd4);
}

.btn-glow {
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, rgba(255,255,255,0.15), transparent);
  pointer-events: none;
}

/* Loading dots */
.dots {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
  height: 20px;
}
.dots span {
  width: 6px; height: 6px;
  background: white;
  border-radius: 50%;
  animation: dot-bounce 1.2s ease-in-out infinite;
}
.dots span:nth-child(2) { animation-delay: 0.2s; }
.dots span:nth-child(3) { animation-delay: 0.4s; }
@keyframes dot-bounce {
  0%, 60%, 100% { transform: translateY(0); opacity: 0.7; }
  30% { transform: translateY(-6px); opacity: 1; }
}

/* Responsive */
@media (max-width: 600px) {
  .form-row { grid-template-columns: 1fr; }
  .card-inner { padding: 28px 20px; }
  .form-footer { flex-direction: column-reverse; align-items: stretch; }
  .submit-btn { width: 100%; }
  .social-name { display: inline; }
  .content-wrapper { padding: 32px 16px; }
}
</style>