<template>
  <div>
    <h1>- Zonas Arqueológicas -</h1>
    <button class="btnNuevo" @click="agregarZona">Agregar nueva zona</button>
    <ul class="zonas-list">
      <ZonasItem
        v-for="zona in uniqueZonas"
        :key="zona.id"
        v-bind="zona"
        @eliminarZona="eliminarZona"
      />
    </ul>
  </div>
</template>

<script setup lang="ts">
import ZonasData from '@/data/ZonasData';
import ZonasItem from './ZonasItem.vue';
import { ref } from 'vue';

interface IZonas {
  id: number;
  titulo: string;
  descripcion: string;
  disponible: boolean;
  precio: number;
  link_img: string;
}

const uniqueZonas = ref<IZonas[]>(ZonasData);
let ultimoId = uniqueZonas.value.length;

const agregarZona = () => {
  const titulo = prompt('Ingrese el título de la nueva zona:') || '';
  const descripcion = prompt('Ingrese la descripcion:') || '';
  const disponibilityInput = prompt('¿Se encuentra abierta al público (Si/No)?: ') || '';
  const priceInput = prompt('Ingrese el precio de la entrada:') || '';
  const imageInput = prompt('Ingrese el link de la imagen:') || '';

  const errores: string[] = [];

  if (!validarInput(titulo, 'string')) {
    errores.push('El título ingresado no es válido.');
  }

  if (!validarDisponibilidad(disponibilityInput)) {
    errores.push('La disponibilidad ingresada debe ser "Si" o "No".');
  }

  if (!validarInput(priceInput, 'number')) {
    errores.push('El precio ingresado debe ser un número válido.');
  }

  if (errores.length > 0) {
    alert(errores.join('\n'));
    return;
  }

  const disponible = disponibilityInput.toLowerCase() === 'si';
  const precio = parseFloat(priceInput);
  const link_img = imageInput;

  if (tituloDuplicado(titulo)) {
    alert('Ya existe una zona arqueológica con el mismo nombre, por lo tanto no se agregará.');
    return;
  }

  const nuevaZona: IZonas = {
    id: ++ultimoId,
    titulo,
    descripcion,
    disponible,
    precio,
    link_img,
  };

  uniqueZonas.value = [...uniqueZonas.value, nuevaZona];
};

const eliminarZona = (zonaId: number) => {
  uniqueZonas.value = uniqueZonas.value.filter(zona => zona.id !== zonaId);
};

const validarInput = (input: string, type: string): boolean => {
  if (type === 'string') {
    const regex = /^[a-zA-Z\s]+$/;
    return regex.test(input);
  } else if (type === 'number') {
    return !isNaN(parseFloat(input)) && isFinite(parseFloat(input));
  }
  return true;
};

const validarDisponibilidad = (input: string): boolean => {
  const inputMinuscula = input.toLowerCase();
  return inputMinuscula === 'si' || inputMinuscula === 'no';
};

const tituloDuplicado = (titulo: string): boolean => {
  return uniqueZonas.value.some(zona => zona.titulo === titulo);
}

</script>

<style scoped>

h1{
  text-align: center;
  color: #4caf50;
  font-weight: bold;
  padding: 0;
  margin: 0;
}
.zonas-list {
  list-style: none;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 10px;
}

.btnNuevo{
    display: block;
    margin: 10px auto;
    padding: 5px 10px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 10px;
    cursor: pointer;
  }

@media (max-width: 768px) {
  .zonas-list {
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 480px) {
  .zonas-list {
    grid-template-columns: 1fr;
  }
}
</style>