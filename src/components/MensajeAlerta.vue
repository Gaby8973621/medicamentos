<template>
  <div class="alert-container">
    <p>{{ mensajeALERTA }}</p>
  </div>
</template>

<script>
export default {
  name: 'MensajeAlerta',
  props: {
    mensajeALERTA: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
.alert-container {
  padding: 10px;
  border-radius: 5px;
  background-color: #f44336; /* Cambia el color según tu preferencia */
  color: white;
}
</style>
