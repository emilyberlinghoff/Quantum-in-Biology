<template>
  <div class="noise-card">
    <div class="noise-title">Noise vs. Energy Estimation</div>

    <div class="noise-row">
      <label class="noise-label">Noise level</label>
      <input
        class="noise-slider"
        type="range"
        min="0"
        max="0.5"
        step="0.01"
        v-model.number="noise"
      />
      <div class="noise-value">{{ noise.toFixed(2) }}</div>
    </div>

    <div class="noise-row">
      <label class="noise-label">Measured energy</label>
      <div class="noise-value">{{ measured.toFixed(4) }}</div>
    </div>

    <div class="noise-chart">
      <svg viewBox="0 0 320 140" aria-label="Noise curve">
        <polyline :points="idealPoints" fill="none" stroke="#7bd3ff" stroke-width="2" />
        <polyline :points="noisyPoints" fill="none" stroke="#ff7bd4" stroke-width="2" />
      </svg>
      <div class="noise-legend">
        <span class="legend-item"><span class="dot ideal"></span>Ideal</span>
        <span class="legend-item"><span class="dot noisy"></span>Noisy</span>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

const noise = ref(0.15)

const idealFn = (x: number) => {
  const a = 1.8
  const re = 0.9
  const De = 1.0
  return De * (1 - Math.exp(-a * (x - re))) ** 2 + 0.15
}

const measured = computed(() => idealFn(0.9) + (noise.value * 0.15))

const idealPoints = computed(() => {
  const pts: string[] = []
  for (let i = 0; i <= 80; i++) {
    const x = 0.4 + (2.5 - 0.4) * (i / 80)
    const e = idealFn(x)
    const px = (i / 80) * 320
    const py = 120 - (e - 0.15) * 90
    pts.push(`${px.toFixed(1)},${py.toFixed(1)}`)
  }
  return pts.join(' ')
})

const noisyPoints = computed(() => {
  const pts: string[] = []
  for (let i = 0; i <= 80; i++) {
    const x = 0.4 + (2.5 - 0.4) * (i / 80)
    const base = idealFn(x)
    const wobble = Math.sin(i / 6) * noise.value * 0.25
    const e = base + wobble
    const px = (i / 80) * 320
    const py = 120 - (e - 0.15) * 90
    pts.push(`${px.toFixed(1)},${py.toFixed(1)}`)
  }
  return pts.join(' ')
})
</script>

<style scoped>
.noise-card {
  background: #101a28;
  border: 1px solid rgba(123, 211, 255, 0.28);
  border-radius: 16px;
  padding: 20px;
  max-width: 720px;
  margin: 0 auto;
}

.noise-title {
  font-size: 1.2rem;
  margin-bottom: 12px;
  color: #cfe8ff;
}

.noise-row {
  display: flex;
  align-items: center;
  gap: 12px;
  margin: 8px 0;
}

.noise-label {
  width: 140px;
  color: #a8b4c3;
}

.noise-slider {
  flex: 1;
}

.noise-value {
  width: 80px;
  text-align: right;
  color: #ffffff;
}

.noise-chart {
  margin-top: 8px;
}

.noise-chart svg {
  width: 100%;
  height: 160px;
}

.noise-legend {
  display: flex;
  gap: 12px;
  font-size: 0.85rem;
  color: #a8b4c3;
  margin-top: 6px;
}

.legend-item {
  display: inline-flex;
  align-items: center;
  gap: 6px;
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  display: inline-block;
}

.dot.ideal {
  background: #7bd3ff;
}

.dot.noisy {
  background: #ff7bd4;
}

.noise-note {
  margin-top: 8px;
  color: #a8b4c3;
  font-size: 0.9rem;
}
</style>
