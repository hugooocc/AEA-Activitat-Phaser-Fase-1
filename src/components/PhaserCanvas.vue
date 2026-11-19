<script setup>
// 1. Importem ref (referència al DOM), watch i els hooks del cicle de vida de Vue 3
import { ref, watch, onMounted, onUnmounted } from 'vue';
// 2. Importem el motor Phaser 4 i la nostra escena personalitzada
import Phaser from 'phaser';
import { MainScene } from '../game/MainScene';

// Props que rebem des d'App.vue
const props = defineProps({
  nomUsuari: { type: String, default: 'Anna' },
  colorAvatar: { type: String, default: '#10b981' }, // [REPTE 1]
  estat: { type: String, default: 'Disponible' },     // [REPTE 2]
  pausat: { type: Boolean, default: false }           // [REPTE 3]
});

const contenidor = ref(null); // Apuntarà al <div ref="contenidor"> de sota
let game = null;              // Guardem la instància per destruir-la en sortir

// Recupera l'escena activa (undefined si el joc encara no existeix)
const obtenirEscena = () => game?.scene.getScene('MainScene');

// Aplica TOTES les props a l'escena. Útil amb v-if: si canvies valors mentre
// el llenç està amagat, en tornar-lo a mostrar arrenca ja amb els valors nous.
function sincronitzarEscena() {
  const escena = obtenirEscena();
  if (!escena) return;
  escena.actualitzarNom(props.nomUsuari);
  escena.canviarColor(props.colorAvatar);
  escena.actualitzarEstat(props.estat);
  if (props.pausat) escena.scene.pause();
}

// S'executa un sol cop quan el <div> ja existeix a la pàgina
onMounted(() => {
  game = new Phaser.Game({
    type: Phaser.AUTO,          // Tria WebGL automàticament (o Canvas 2D)
    width: 640, height: 400,    // Mides del llenç en píxels
    backgroundColor: '#1e293b', // Fons fosc per distingir l'àrea del joc
    parent: contenidor.value,   // Injecta el <canvas> dins del nostre <div>
    scene: [MainScene]          // Escena inicial que arrencarà el motor
  });

  // Sincronitza l'estat actual de Vue només acabar d'arrencar el motor
  game.events.once('ready', sincronitzarEscena);
});

// IMPRESCINDIBLE: quan s'amaga (v-if="false"), atura el bucle a 60 FPS
onUnmounted(() => {
  game?.destroy(true);
  game = null;
});

// Watchers al MATEIX nivell que onMounted (MAI dins d'onMounted)
watch(() => props.nomUsuari, (nouNom) => {
  obtenirEscena()?.actualitzarNom(nouNom);
});

// [REPTE 1] Segon watch(): color de l'avatar
watch(() => props.colorAvatar, (nouColor) => {
  obtenirEscena()?.canviarColor(nouColor);
});

// [REPTE 2] Estat de l'usuari
watch(() => props.estat, (nouEstat) => {
  obtenirEscena()?.actualitzarEstat(nouEstat);
});

// [REPTE 3] Pausa / Reprèn el Game Loop de l'escena
watch(() => props.pausat, (pausa) => {
  const escena = obtenirEscena();
  if (!escena) return;
  if (pausa) escena.scene.pause();
  else escena.scene.resume();
});
</script>

<template>
  <!-- Aquí s'injecta el <canvas> -->
  <div ref="contenidor" class="llenc"></div>
</template>

<style scoped>
.llenc {
  line-height: 0; /* evita l'espai blanc sota el <canvas> */
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgb(0 0 0 / 0.35);
}
</style>
