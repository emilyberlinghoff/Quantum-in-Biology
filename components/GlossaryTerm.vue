<template>
  <button class="glossary-pill" :class="{ open }" @click="open = !open">
    <span class="term">{{ term }}</span>
    <span v-if="open" class="def" v-html="definition"></span>
  </button>
</template>

<script setup lang="ts">
import { ref } from 'vue'

defineProps<{
  term: string
  definition: string
}>()

const open = ref(false)
</script>

<style scoped>
.glossary-pill {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 8px 12px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(123, 211, 255, 0.25);
  color: #e9eff7;
  cursor: pointer;
  font-size: 1.1rem;
  line-height: 1.25;
  text-align: left;
  width: fit-content;
  max-width: 100%;
}

.term {
  font-weight: 600;
  color: #cfe8ff;
}

.def {
  color: #a8b4c3;
}

.glossary-pill.open {
  background: rgba(123, 211, 255, 0.12);
  border-color: rgba(123, 211, 255, 0.45);
}
</style>
