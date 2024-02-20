<template>
    <div class="card" :style="{ backgroundColor: backgroundColor, color: textColor, border: border}">
      <span>
        <p class="titulo" :style="{ backgroundColor: textColor, color: backgroundColor }">{{ props.titulo }}</p>
        <p>Zona No.#{{ props.id }}</p>
        <p><span class="descripcion">Significado:</span>{{ props.descripcion }}</p>
        <p><span class="disponible">¿Abierto al público?:</span> {{ props.disponible }}</p>
        <p><span class="precio">Precio entrada:</span> ${{ props.precio }}</p>
        <img class="img" v-if="props.link_img" :src="props.link_img" width="150px" />
      </span>
      <span>
        <p>Fondo: <input type="color" v-model="backgroundColor"/></p>
        <p>Texto: <input type="color" v-model="textColor"/></p>
      </span>
      <button class="btnEliminar" @click="confirmarEliminar">Eliminar zona</button>
    </div>
  </template>
  
  <script setup lang="ts">
  import type { IZonas } from '@/interfaces/IZonas';
  import { ref } from 'vue';
  
  const props = defineProps<IZonas>();
  const emits = defineEmits(['eliminarZona']);
  
  const backgroundColor = ref('');
  const textColor = ref('');
  const border = ref('1px solid');

  const confirmarEliminar = () => {
  const confirmar = window.confirm(`¿Seguro que desea eliminar la zona: ${props.titulo}?`);

  if (confirmar) {
    eliminarZona();
    alert(`La zona "${props.titulo}" ha sido eliminada.`);
  }
};

const eliminarZona = () => {
  emits('eliminarZona', props.id);
};

</script>
  
<style scoped>
  .card {
    border-radius: 10px;
    margin: 20px;
    width: 200px;
    transition: background-color 0.3s, color 0.3s;
  }

</style>