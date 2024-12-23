<!-- (SFC) Es una convencion que existe en VueJS donde un componente contiene 3 partes, SCRIP, TEMPLATE y STYLE-->
<!-- EL SETUP ES PARA ACTIVAR COMPOSITION API EN EL ARCHIVO -->
<script setup>
import { ref, reactive, onMounted } from "vue";
import { db } from "./data/guitarras";
import Guitarra from "./components/Guitarra.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
// AQUI IRA TODA LA LOGICA E IMPORTACIONES DE JAVASCRIPT
// const hola = "Hola mundo";

// Utilizando reactive
// const state = reactive({
//   guitarras: [],
// });

// Utilizando ref
const guitarras = ref([]);
const carrito = ref([]);
const guitarra = ref({});

onMounted(() => {
  guitarras.value = db;
  guitarra.value = db[3];
  // state.guitarras = db;
});

// console.log("Imprimiendo desde REACTIVE:", state.guitarras);
// console.log("Imprimiendo desde REF:", guitarras.value);

const agregarCarrito = (guitarra) => {
  // console.log("Agregando...", guitarra);
  const existeCarrito = carrito.value.findIndex(
    (producto) => producto.id === guitarra.id
  );
  if (existeCarrito >= 0) {
    carrito.value[existeCarrito].cantidad++;
  } else {
    guitarra.cantidad = 1;
    carrito.value.push(guitarra);
  }
};

const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad <= 1) return;
  carrito.value[index].cantidad--;
};

const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex((producto) => producto.id === id);
  if (carrito.value[index].cantidad >= 5) return;
  carrito.value[index].cantidad++;
};

const eliminarProducto = (id) => {
  carrito.value = carrito.value.filter((producto) => producto.id !== id);
};

const vaciarCarrito = () => {
  carrito.value = [];
};
</script>

<!-- SINGLE FILE COMPONENTS -->
<!-- Basicamente es el HTML, CSS Y JAVASCRIPT en un solo archivo -->
<template>
  <!-- AQUI IRA TODA LA PRESENTACION, ES DECIR TODO EL CODIGO HTML QUE IRA EN PANTALLA -->

  <!-- Para mandar a llamar las variables en codigo HTML se utiliza doble llave-->
  <!-- <h1>{{ hola }}</h1> -->
  <!-- ":" Si tiene dos puntos es un prop y si tiene el "@" es un evento -->
  <Header
    :carrito="carrito"
    :guitarra="guitarra"
    @incrementar-cantidad="incrementarCantidad"
    @decrementar-cantidad="decrementarCantidad"
    @agregar-carrito="agregarCarrito"
    @eliminar-producto="eliminarProducto"
    @vaciar-carrito="vaciarCarrito"
  />
  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>
    <!-- Pasando funcion al coomponente -->
    <div class="row mt-5">
      <Guitarra
        v-for="guitarra in guitarras"
        :guitarra="guitarra"
        @agregar-carrito="agregarCarrito"
      />
      <!-- FIN GUITARRA -->
    </div>
  </main>

  <Footer />
</template>

<style scoped>
/* AQUI IRA TODOS LOS ESTILOS CSS O TAMBIEN CODIGO SASS */
/* h1 {
  color: red;
  text-transform: uppercase;
} */
</style>
