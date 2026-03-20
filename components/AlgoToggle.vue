<template>
  <div class="toggle-card">
    <div class="toggle-header">
      <div class="toggle-title">VQE vs QPE</div>
      <div class="toggle-switch">
        <button
          class="toggle-btn"
          :class="{ active: mode === 'vqe' }"
          @click="mode = 'vqe'"
        >VQE</button>
        <button
          class="toggle-btn"
          :class="{ active: mode === 'qpe' }"
          @click="mode = 'qpe'"
        >QPE</button>
      </div>
    </div>

    <div class="toggle-body">
      <div>
        <div class="section-title">Pros</div>
        <ul>
          <li v-for="p in pros" :key="p">{{ p }}</li>
        </ul>

        <div class="section-title">Cons</div>
        <ul>
          <li v-for="c in cons" :key="c">{{ c }}</li>
        </ul>
      </div>

      <div class="depth-panel">
        <div class="section-title">Circuit Depth</div>
        <div class="depth-bar">
          <div class="depth-fill" :style="{ width: depth + '%', background: depthGradient }"></div>
        </div>
        <div class="depth-label">{{ depthLabel }}</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

const mode = ref<'vqe' | 'qpe'>('vqe')

const pros = computed(() =>
  mode.value === 'vqe'
    ? [
        'Works on noisy (NISQ) hardware',
        'Shallow circuits',
        'Flexible ansatz choices',
      ]
    : [
        'Higher theoretical precision',
        'Direct eigenvalue estimation',
        'No variational optimization',
      ]
)

const cons = computed(() =>
  mode.value === 'vqe'
    ? [
        'Optimization can get stuck',
        'Accuracy depends on ansatz',
        'Many measurements needed',
      ]
    : [
        'Deep circuits (noise sensitive)',
        'Requires long coherence times',
        'Harder on current hardware',
      ]
)

const depth = computed(() => (mode.value === 'vqe' ? 35 : 85))
const depthLabel = computed(() => (mode.value === 'vqe' ? 'Shallow' : 'Deep'))
const depthGradient = computed(() => {
  const left = '#7bd3ff'
  const right = '#7b6dff'
  return `linear-gradient(90deg, ${left}, ${right})`
})
</script>

<style scoped>
.toggle-card {
  background: #101a28;
  border: 1px solid rgba(123, 211, 255, 0.28);
  border-radius: 16px;
  padding: 20px;
  max-width: 820px;
  margin: 0 auto;
}

.toggle-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.toggle-title {
  color: #cfe8ff;
  font-size: 1.2rem;
}

.toggle-switch {
  display: inline-flex;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(123, 211, 255, 0.2);
  border-radius: 999px;
  overflow: hidden;
}

.toggle-btn {
  padding: 6px 12px;
  background: transparent;
  color: #cfe8ff;
  border: none;
  cursor: pointer;
}

.toggle-btn.active {
  background: linear-gradient(90deg, #2ee6a6, #7bd3ff);
  color: #08121b;
  font-weight: 600;
}

.toggle-body {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 16px;
}

.section-title {
  color: #a8b4c3;
  font-size: 0.9rem;
  margin-top: 8px;
}

.depth-panel {
  background: rgba(255, 255, 255, 0.04);
  border: 1px solid rgba(123, 211, 255, 0.14);
  border-radius: 12px;
  padding: 12px;
}

.depth-bar {
  height: 10px;
  background: rgba(255, 255, 255, 0.08);
  border-radius: 999px;
  overflow: hidden;
  margin: 6px 0;
}

.depth-fill {
  height: 100%;
}

.depth-label {
  color: #cfe8ff;
  font-size: 0.9rem;
}
</style>
