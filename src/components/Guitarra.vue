<script setup>
import { ref } from "vue";
const numero = ref(0);

// Utilizando macro
const props = defineProps({
  guitarra: {
    type: Object,
    required: true,
  },
});

defineEmits(["agregar-carrito"]);
</script>

<template>
  <div class="col-md-6 col-lg-4 my-4 row align-items-center">
    <div class="col-4">
      <!-- Concatenando  nombres de archivos y alt dinamicos-->
      <img
        class="img-fluid"
        v
        :src="'/img/' + guitarra.imagen + '.jpg'"
        :alt="'Imagen de guitarra' + guitarra.nombre"
      />
    </div>
    <div class="col-8">
      <h3 class="text-black fs-4 fw-bold text-uppercase">
        {{ guitarra.nombre }}
      </h3>
      <p>{{ numero }}</p>
      <p>
        {{ guitarra.descripcion }}
      </p>
      <p class="fw-black text-primary fs-3">{{ guitarra.precio }}</p>
      <!-- En view existen 2 tipos de eventos, los inlinehandlers y los methodHandlers -->
      <button
        @click="$emit('agregar-carrito', guitarra)"
        type="button"
        class="btn btn-dark w-100"
      >
        Agregar al Carrito
      </button>
    </div>
  </div>
</template>

<style></style>
