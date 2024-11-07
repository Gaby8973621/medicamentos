<template>
    <div>
      <h2>Agregar Medicamento</h2>
      <form @submit.prevent="submit">
        <div>
          <label for="nombre">Nombre:</label>
          <input v-model="nombre" id="nombre" required />
        </div>
        <div>
          <label for="cantidad">Cantidad:</label>
          <input type="number" v-model="cantidad" id="cantidad" min="1" required />
        </div>
        <button type="submit">Agregar Medicamento</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        nombre: '',
        cantidad: 1,
      };
    },
    methods: {
      submit() {
        // Emitir el evento con el nuevo medicamento
        this.$emit('agregar-medicamento', {
          id: Date.now(), // Genera un ID único
          nombre: this.nombre,
          cantidad: this.cantidad,
        });
        // Restablecer los campos del formulario
        this.nombre = '';
        this.cantidad = 1;
      },
    },
  };
  </script>
  
  <style scoped>
  /* Estilos para el formulario */
  </style>
  