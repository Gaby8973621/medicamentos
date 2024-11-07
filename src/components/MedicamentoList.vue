<template>
  <div>
    <h3>Lista de Medicamentos</h3>
    <ul>
      <li v-for="medicamento in medicamentos" :key="medicamento.id">
        {{ medicamento.nombre }} - Cantidad: {{ medicamento.cantidad }}
        <button @click="removeMedicamento(medicamento.id)">Eliminar</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'MedicamentoList',
  props: {
    medicamentos: Array,
  },
  methods: {
    removeMedicamento(id) {
      this.$emit('remove', id);
    }
  },
};
</script>

<style scoped>
/* Añade estilos aquí si es necesario */
</style>
