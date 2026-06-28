<script setup lang="ts">
import { ref, onMounted, onUnmounted, computed } from 'vue'

// ── Event date ──────────────────────────────────────────────
const EVENT_DATE = new Date('2026-07-05T17:00:00')

// ── State ───────────────────────────────────────────────────
const now   = ref(new Date())
let   timer = 0

onMounted(() => {
  timer = window.setInterval(() => { now.value = new Date() }, 1000)
})

onUnmounted(() => clearInterval(timer))

// ── Derived ─────────────────────────────────────────────────
const diff = computed(() => Math.max(0, EVENT_DATE.getTime() - now.value.getTime()))

const days  = computed(() => Math.floor(diff.value / 86_400_000))
const hours = computed(() => Math.floor((diff.value % 86_400_000) / 3_600_000))
const mins  = computed(() => Math.floor((diff.value % 3_600_000)  / 60_000))
const secs  = computed(() => Math.floor((diff.value % 60_000)     / 1_000))

const isDone = computed(() => diff.value === 0)

function pad(n: number) { return String(n).padStart(2, '0') }

// Text shown inside the TV screen
const tvLabel = computed(() => {
  if (isDone.value) return '¡EL SHOW ES HOY!'
  if (days.value === 0) return '¡HOY EMPIEZA!'
  return `FALTAN ${days.value} DÍA${days.value !== 1 ? 'S' : ''}...`
})
</script>

<template>
  <div class="countdown-root">

    <!-- TV-screen label (parents can pick this up via $refs if needed) -->
    <p class="tv-label font-display" :class="{ done: isDone }">
      {{ tvLabel }}
    </p>

    <!-- Countdown blocks -->
    <div class="blocks" role="timer" aria-label="Cuenta regresiva al show">
      <div class="block">
        <span class="num font-display">{{ pad(days) }}</span>
        <span class="lbl">días</span>
      </div>
      <span class="sep font-display" aria-hidden="true">:</span>
      <div class="block">
        <span class="num font-display">{{ pad(hours) }}</span>
        <span class="lbl">horas</span>
      </div>
      <span class="sep font-display" aria-hidden="true">:</span>
      <div class="block">
        <span class="num font-display">{{ pad(mins) }}</span>
        <span class="lbl">min</span>
      </div>
      <span class="sep font-display" aria-hidden="true">:</span>
      <div class="block">
        <span class="num font-display">{{ pad(secs) }}</span>
        <span class="lbl">seg</span>
      </div>
    </div>

  </div>
</template>

<style scoped>

.countdown-root {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; /* Centra el reloj verticalmente */
  gap: 0.4rem;
  width: 100%;
  height: 100%; /* Obliga al componente a llenar la pantalla */
}

/* ── TV label ── */
.tv-label {
  font-size: clamp(0.5rem, 3.5vw, 1.75rem);
  color: var(--circus-yellow);
  letter-spacing: 0.1em;
  text-shadow:
    0 0 12px color-mix(in srgb, var(--circus-yellow) 70%, transparent),
    1px 1px 0 #7a4400;
  text-align: center;
  transition: color 0.4s;
}
.tv-label.done {
  color: var(--circus-green);
  animation: pulse-glow 1s ease-in-out infinite;
  --glow-color: var(--circus-green);
}

/* ── Blocks ── */
.blocks {
  display: flex;
  align-items: center;
  gap: clamp(1px, 0.1vw, 1px);
}

.block {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: rgba(255, 255, 255, 0.06);
  border: 1.5px solid rgba(241, 196, 14, 0.25);
  border-radius: 8px;
  padding: clamp(2px, 1vw, 10px) clamp(4px, 1.5vw, 16px);
  /* CORRECCIÓN: valor mínimo positivo */
  min-width: clamp(30px, 10vw, 60px); 
  backdrop-filter: blur(6px);
}

.num {
  font-size: clamp(0.5rem, 2.5vw, 1.5rem);
  line-height: 0.5;
  color: var(--circus-yellow);
  text-shadow:
    0 0 16px color-mix(in srgb, var(--circus-yellow) 60%, transparent),
    1px 1px 0 #7a4400;
  animation: pulse-glow 1.5s ease-in-out infinite;
  --glow-color: var(--circus-yellow);
}

.lbl {
  font-size: clamp(0.1rem, 1.5vw, 0.75rem);
  font-weight: 700;
  letter-spacing: 0.08em;
  color: rgba(255, 255, 255, 0.45);
  text-transform: uppercase;
  margin-top: 2px;
}

.sep {
  font-size: clamp(0.1rem, 2vw, 2rem);
  color: rgba(241, 196, 14, 0.4);
  padding-bottom: 1rem;
  animation: blink 1s step-end infinite;
}
</style>
