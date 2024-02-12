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
  
  const backgroundColor = ref('#FFFFFF');
  const textColor = ref('#4caf50');
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

  .titulo{
    text-align: center;
    font-weight: bold;
    border-radius: 10px 10px 0 0;
    padding: 0;
    margin: 0;
  }

  .disponible, .precio, .descripcion{
    font-weight: bold;
    text-align: justify;
  }

  .img {
  display: block;
  margin: 0 auto;
  max-width: 100%;
  height: auto;
  margin-top: 10px; 
  border-radius: 15px;
  padding: 10px 0;
  }

  .btn{
    display: block;
    margin: 10px auto;
    padding: 5px 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 10px;
    cursor: pointer;
  }

  p{
    padding: 0 10px;
  }

.btnEliminar {
  display: block;
    margin: 10px auto; 
    padding: 5px 10px;
    background-color: #ff3636;
    color: white;
    border: none;
    border-radius: 10px;
    cursor: pointer;
}
</style>