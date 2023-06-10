<script setup>
import { ref, computed } from 'vue';

const nombre = ref("");
const unitario = ref("");
const cantidad = ref("");
const listaCompras = ref([]);
const editableCantidad = ref(true);

const agregar = () => {
  const newUser = {
    nombre: nombre.value,
    unitario: unitario.value,
    cantidad: cantidad.value,
    total: unitario.value * cantidad.value
  };

  // Agregar el nuevo usuario a la lista de compras
  listaCompras.value.push(newUser);

  // Reiniciar los campos del formulario
  nombre.value = "";
  unitario.value = "";
  cantidad.value = "";
};

// Propiedad computada para calcular el valor total de la compra de manera reactiva
const valorTotalCompra = computed(() => {
  return listaCompras.value.reduce((acc, compra) => acc + compra.total, 0);
});

// Función para actualizar el total cuando se modifica la cantidad
const actualizarTotal = (compra) => {
  compra.total = compra.unitario * compra.cantidad;
};


</script>

<template>
  <div>
    <h3>Carrito de compras</h3><br><br>
    <h4>Nombre</h4>
    <input type="text" v-model="nombre" required>
    <h4>Valor Unitario</h4>
    <input type="text" v-model="unitario">
    <h4>Cantidad</h4>
    <input type="number" v-model="cantidad" :disabled="!editableCantidad" @input="actualizarTotal(compra)">
    <h7></h7><br><br>
    <button class="button" type="submit" @click="agregar">Agregar</button>
  </div>

  <div class="carrito">
    <table>
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Valor Unitario</th>
          <th>Cantidad</th>
          <th>Total</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="compra in listaCompras" :key="compra.nombre">
          <td><li>{{ compra.nombre }}</li></td>
          <td>{{ compra.unitario }}</td>
          <td>
            <input type="number" v-model="compra.cantidad" :disabled="!editableCantidad" @input="actualizarTotal(compra)">
          </td>
          <td>{{ compra.total }}</td>
        </tr>
      </tbody>
    </table>
    <p>Total de la compra: {{ valorTotalCompra }}</p>
   
  </div>
</template>

<style scoped>
  .carrito{
    color: black;
    text-align: center;
    margin: 10px;
    background-color: white;
    border-radius: 10px;
    border-color: red 3px;
  }
</style>
