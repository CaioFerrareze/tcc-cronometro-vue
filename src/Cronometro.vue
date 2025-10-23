<template>
  <div style="border:1px solid #ccc;padding:16px;border-radius:8px;width:240px">
    <h3>{{ formatarTempo(tempo) }}</h3>
    <div style="display:flex;gap:8px;justify-content:center">
      <button @click="iniciarPausar">{{ rodando ? "Pausar" : "Iniciar" }}</button>
      <button @click="zerar">Zerar</button>
      <button @click="registrarVolta">Volta</button>
    </div>
    <ul style="text-align:left;margin-top:10px">
      <li v-for="(v,i) in voltas" :key="i">
        Volta {{ i + 1 }}: {{ formatarTempo(v) }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onBeforeUnmount, watch } from "vue";

const props = defineProps({
  formato: String,
  voltas: Array
});
const emit = defineEmits(["updateVoltas"]);

const tempo = ref(0);
const rodando = ref(false);
let intervalo = null;

watch(rodando, (ativo) => {
  if (ativo) {
    intervalo = setInterval(() => (tempo.value += 100), 100);
  } else {
    clearInterval(intervalo);
  }
});

onBeforeUnmount(() => clearInterval(intervalo));

function formatarTempo(ms) {
  const totalSeg = Math.floor(ms / 1000);
  const horas = Math.floor(totalSeg / 3600);
  const minutos = Math.floor((totalSeg % 3600) / 60);
  const segundos = totalSeg % 60;
  if (props.formato === "hh:mm:ss")
    return `${String(horas).padStart(2, "0")}:${String(minutos).padStart(
      2,
      "0"
    )}:${String(segundos).padStart(2, "0")}`;
  return `${String(minutos).padStart(2, "0")}:${String(segundos).padStart(
    2,
    "0"
  )}`;
}

function iniciarPausar() {
  rodando.value = !rodando.value;
}
function zerar() {
  tempo.value = 0;
}
function registrarVolta() {
  emit("updateVoltas", [...(props.voltas || []), tempo.value]);
}
</script>
