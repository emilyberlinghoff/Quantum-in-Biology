<template>
  <div class="curve-card">
    <div class="curve-title">H₂ Bond‑Length Energy Curve</div>

    <div class="curve-row">
      <label class="curve-label">Bond length r (Å)</label>
      <input
        class="curve-slider"
        type="range"
        min="0.4"
        max="2.5"
        step="0.01"
        v-model.number="r"
      />
      <div class="curve-value">{{ r.toFixed(2) }}</div>
    </div>

    <div class="curve-row">
      <label class="curve-label">Energy E(r)</label>
      <div class="curve-value">{{ energy.toFixed(4) }}</div>
    </div>

    <svg class="curve-chart" viewBox="0 0 320 140" aria-label="Energy curve">
      <polyline :points="curvePoints" fill="none" stroke="#7bd3ff" stroke-width="2" />
      <circle :cx="markerX" :cy="markerY" r="4" fill="#7b6dff" />
      <line :x1="markerX" y1="0" :x2="markerX" y2="140" stroke="rgba(255,255,255,0.1)" stroke-dasharray="4 4" />
    </svg>

    <div class="curve-note">
      Minimum energy occurs near the equilibrium bond length
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

const r = ref(0.9)

const energyFn = (x: number) => {
  // Simplified Morse-like curve (not physically accurate)
  const a = 1.8
  const re = 0.9
  const De = 1.0
  return De * (1 - Math.exp(-a * (x - re))) ** 2 + 0.15
}

const energy = computed(() => energyFn(r.value))

const curvePoints = computed(() => {
  const pts: string[] = []
  for (let i = 0; i <= 80; i++) {
    const x = 0.4 + (2.5 - 0.4) * (i / 80)
    const e = energyFn(x)
    const px = (i / 80) * 320
    const py = 120 - (e - 0.15) * 90
    pts.push(`${px.toFixed(1)},${py.toFixed(1)}`)
  }
  return pts.join(' ')
})

const markerX = computed(() => ((r.value - 0.4) / (2.5 - 0.4)) * 320)
const markerY = computed(() => 120 - (energy.value - 0.15) * 90)
</script>

<style scoped>
.curve-card {
  background: #101a28;
  border: 1px solid rgba(123, 211, 255, 0.28);
  border-radius: 16px;
  padding: 20px;
  max-width: 720px;
  margin: 0 auto;
}

.curve-title {
  font-size: 1.2rem;
  margin-bottom: 12px;
  color: #cfe8ff;
}

.curve-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 8px 0;
}

.curve-label {
  width: 180px;
  color: #a8b4c3;
}

.curve-slider {
  flex: 1;
}

.curve-value {
  width: 80px;
  text-align: right;
  color: #ffffff;
}

.curve-chart {
  width: 100%;
  height: 160px;
  margin: 10px 0 4px;
}

.curve-note {
  margin-top: 6px;
  color: #a8b4c3;
  font-size: 0.9rem;
}
</style>
