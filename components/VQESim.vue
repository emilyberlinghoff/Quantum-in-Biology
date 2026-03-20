<template>
  <div class="vqe-card">
    <div class="vqe-title">VQE Loop Simulator</div>
    <div class="vqe-row">
      <label class="vqe-label">Parameter θ</label>
      <input
        class="vqe-slider"
        type="range"
        min="0"
        max="3.14"
        step="0.01"
        v-model.number="theta"
      />
      <div class="vqe-value">{{ theta.toFixed(2) }}</div>
    </div>

    <div class="vqe-row">
      <label class="vqe-label">Energy E(θ)</label>
      <div class="vqe-value">{{ energy.toFixed(4) }}</div>
    </div>

    <div class="vqe-bar">
      <div class="vqe-bar-fill" :style="{ width: energyPercent + '%' }"></div>
    </div>

    <svg class="vqe-chart" viewBox="0 0 300 120" aria-label="Energy curve">
      <polyline :points="curvePoints" fill="none" stroke="#7bd3ff" stroke-width="2" />
      <circle :cx="markerX" :cy="markerY" r="4" fill="#7b6dff" />
    </svg>

    <div class="vqe-actions">
      <button class="vqe-btn" @click="step">Optimize Step</button>
      <button class="vqe-btn" @click="reset">Reset</button>
    </div>

    <div class="vqe-note">
      This is a simplified model. The “quantum” step is the energy measurement,
      the “classical” step updates θ to lower the energy.
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

const theta = ref(1.6)
const lr = 0.15

const energyFn = (t: number) => {
  const base = (t - 1.25) * (t - 1.25)
  const ripple1 = 0.18 * Math.sin(5 * t)
  const ripple2 = 0.08 * Math.cos(9 * t)
  return 0.55 + base + ripple1 + ripple2
}

const energy = computed(() => energyFn(theta.value))

const grad = (t: number) => {
  const eps = 1e-3
  return (energyFn(t + eps) - energyFn(t - eps)) / (2 * eps)
}

const clamp = (t: number) => Math.max(0, Math.min(3.14, t))

const step = () => {
  theta.value = clamp(theta.value - lr * grad(theta.value))
}

const reset = () => {
  theta.value = 1.6
}

const curvePoints = computed(() => {
  const pts: string[] = []
  for (let i = 0; i <= 60; i++) {
    const t = (3.14 * i) / 60
    const e = energyFn(t)
    const x = (i / 60) * 300
    const y = 110 - (e - 0.5) * 60
    pts.push(`${x.toFixed(1)},${y.toFixed(1)}`)
  }
  return pts.join(' ')
})

const markerX = computed(() => (theta.value / 3.14) * 300)
const markerY = computed(() => 110 - (energy.value - 0.5) * 60)

const energyPercent = computed(() => {
  const e = Math.min(2.0, Math.max(0.5, energy.value))
  return ((e - 0.5) / 1.5) * 100
})
</script>

<style scoped>
.vqe-card {
  background: #101a28;
  border: 1px solid rgba(123, 211, 255, 0.28);
  border-radius: 16px;
  padding: 20px;
  max-width: 720px;
  margin: 0 auto;
}

.vqe-title {
  font-size: 1.2rem;
  margin-bottom: 12px;
  color: #cfe8ff;
}

.vqe-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 8px 0;
}

.vqe-label {
  width: 160px;
  color: #a8b4c3;
}

.vqe-slider {
  flex: 1;
}

.vqe-value {
  width: 80px;
  text-align: right;
  color: #ffffff;
}

.vqe-bar {
  height: 10px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 999px;
  overflow: hidden;
  margin: 10px 0 8px;
}

.vqe-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, #2ee6a6, #7bd3ff, #7b6dff);
}

.vqe-chart {
  width: 100%;
  height: 140px;
  margin: 8px 0;
}

.vqe-actions {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
  margin-top: 10px;
}

.vqe-btn {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(123, 211, 255, 0.2);
  color: #e9eff7;
  padding: 6px 10px;
  border-radius: 10px;
  cursor: pointer;
}

.vqe-note {
  margin-top: 10px;
  color: #a8b4c3;
  font-size: 0.9rem;
}
</style>
