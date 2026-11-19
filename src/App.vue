<script setup>
import { ref } from 'vue';
// Importem el component pont que conté el <canvas> de Phaser
import PhaserCanvas from './components/PhaserCanvas.vue';

// Variable reactiva que guarda el nom escrit a la caixa de text
const nom = ref('Anna');
// Booleà per muntar/desmuntar el joc i verificar onMounted / onUnmounted
const mostrarJoc = ref(true);

// [REPTE 1] Color de l'avatar (format '#rrggbb' de l'<input type="color">)
const color = ref('#10b981');

// [REPTE 2] Estat de l'usuari
const estats = ['Disponible', 'Ocupat', 'Absent'];
const estat = ref('Disponible');

// [REPTE 3] Pausa del Game Loop
const pausat = ref(false);
</script>

<template>
  <main class="contenidor-app">
    <h1>PoC Fase 1 · Vue 3 + Phaser 4</h1>

    <!-- Tota la interfície d'usuari (inputs, botons) es fa en HTML/CSS dins de Vue -->
    <div class="controls">
      <label>
        Nom
        <input v-model="nom" placeholder="Escriu el teu nom d'usuari..." />
      </label>

      <label>
        Color
        <input v-model="color" type="color" />
      </label>

      <label>
        Estat
        <select v-model="estat">
          <option v-for="e in estats" :key="e" :value="e">{{ e }}</option>
        </select>
      </label>

      <button :disabled="!mostrarJoc" @click="pausat = !pausat">
        {{ pausat ? 'Reprèn' : 'Pausa' }}
      </button>

      <button @click="mostrarJoc = !mostrarJoc">
        {{ mostrarJoc ? 'Amaga el llenç' : 'Mostra el llenç' }}
      </button>
    </div>

    <!-- Passem les dades cap avall amb props i controlem el cicle amb v-if -->
    <PhaserCanvas
      v-if="mostrarJoc"
      :nomUsuari="nom"
      :colorAvatar="color"
      :estat="estat"
      :pausat="pausat"
    />
  </main>
</template>

<style scoped>
.contenidor-app {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.25rem;
  padding: 2rem 1rem;
}

h1 {
  margin: 0;
  font-size: 1.5rem;
}

.controls {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: flex-end;
  gap: 0.75rem;
  max-width: 640px;
}

label {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  font-size: 0.8rem;
  color: #94a3b8;
}

input,
select,
button {
  font: inherit;
  font-size: 0.95rem;
  padding: 0.45rem 0.7rem;
  border: 1px solid #334155;
  border-radius: 6px;
  background: #1e293b;
  color: #f1f5f9;
}

input[type='color'] {
  width: 3rem;
  height: 2.3rem;
  padding: 0.15rem;
  cursor: pointer;
}

button {
  cursor: pointer;
  background: #10b981;
  border-color: #10b981;
  color: #0f172a;
  font-weight: 600;
}

button:hover:not(:disabled) {
  filter: brightness(1.1);
}

button:disabled {
  opacity: 0.4;
  cursor: not-allowed;
}
</style>
