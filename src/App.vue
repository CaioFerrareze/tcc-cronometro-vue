<template>
  <div style="text-align:center; font-family:sans-serif; padding:16px">
    <h2>Cronômetro Interativo (Vue)</h2>

    <button @click="alternarFormato">Alternar formato ({{ formato }})</button>
    <button style="margin-left:8px" @click="adicionarCronometro">
      Adicionar Cronômetro
    </button>

    <div style="display:flex;flex-wrap:wrap;justify-content:center;gap:16px;margin-top:16px">
      <Cronometro
        v-for="id in cronometros"
        :key="id"
        :formato="formato"
        :voltas="voltas[id] || []"
        @updateVoltas="(v) => (voltas[id] = v)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Cronometro from "./Cronometro.vue";

const cronometros = ref([0]);
const formato = ref("mm:ss");
const voltas = ref({});

function alternarFormato() {
  formato.value = formato.value === "mm:ss" ? "hh:mm:ss" : "mm:ss";
}
function adicionarCronometro() {
  cronometros.value.push(cronometros.value.length);
}
</script>
