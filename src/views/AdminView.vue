<template>
  <div :class="['admin-container', isDarkMode ? 'dark' : 'light']">
    <h2 class="title">Administrar Medicamentos</h2>

    <!-- Botón de modo claro/oscuro en una esquina con íconos -->
    <button @click="toggleDarkMode" class="theme-toggle-button">
      <img :src="isDarkMode ? 'https://img.icons8.com/ios-glyphs/30/ffffff/new-moon.png' : 'https://img.icons8.com/ios-glyphs/30/000000/sun.png'" alt="Tema" />
    </button>

    <div class="medicamento-card">
      <form @submit.prevent="addMedicamento" class="medicamento-form">
        <div class="form-group">
          <label for="nombre">Nombre del Medicamento:</label>
          <input
            type="text"
            v-model="nuevoMedicamento.nombre"
            id="nombre"
            required
            class="form-input"
          />
        </div>
        <div class="form-group">
          <label for="cantidad">Cantidad:</label>
          <input
            type="number"
            v-model="nuevoMedicamento.cantidad"
            id="cantidad"
            min="1"
            required
            class="form-input cantidad-input"
          />
        </div>
        <div class="form-group">
          <label for="descripcion">Descripción del Medicamento:</label>
          <input
            type="text"
            v-model="nuevoMedicamento.descripcion"
            id="descripcion"
            required
            class="form-input"
          />
        </div>
        <div class="form-group">
          <label for="fechaVencimiento">Fecha de Vencimiento:</label>
          <input
            type="date"
            v-model="nuevoMedicamento.fechaVencimiento"
            id="fechaVencimiento"
            required
            class="form-input"
          />
        </div>
        <button type="submit" class="submit-button">
          <img src="https://img.icons8.com/ios-glyphs/20/ffffff/add.png" alt="Agregar" class="button-icon" />
          Agregar Medicamento
        </button>
      </form>
    </div>

    <div class="table-container">
      <table class="medicamento-table">
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Cantidad</th>
            <th>Descripción</th>
            <th>Fecha de Vencimiento</th>
            <th>Acciones</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="medicamento in medicamentos" :key="medicamento.id">
            <td>
              <img src="https://img.icons8.com/ios-glyphs/20/6f2c91/pill.png" alt="Pastilla" class="pill-icon" />
              <span class="medicamento-nombre">{{ medicamento.nombre }}</span>
            </td>
            <td>
              <span class="cantidad">{{ medicamento.cantidad }}</span>
            </td>
            <td>
              <span class="descripcion">{{ medicamento.descripcion }}</span>
            </td>
            <td>
              <span class="fecha">{{ medicamento.fechaVencimiento }}</span>
            </td>
            <td>
              <button @click="removeMedicamento(medicamento.id)" class="remove-button">
                <img src="https://img.icons8.com/ios-glyphs/20/ffffff/trash.png" alt="Eliminar" class="button-icon" />
                Eliminar
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <h2 class="title">Solicitudes Realizadas</h2>
    <div class="solicitudes-container">
      <button @click="toggleSolicitudes" class="show-solicitudes-button">
        {{ showSolicitudes ? 'Ocultar Solicitudes' : 'Mostrar Solicitudes' }}
      </button>
      <transition name="fade">
        <div v-if="showSolicitudes" class="solicitudes-card">
          <table class="solicitudes-table">
            <thead>
              <tr>
                <th>Nombre Estudiante</th>
                <th>Carrera</th>
                <th>Medicamento</th>
                <th>Cantidad</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(solicitud, index) in solicitudes" :key="index" class="solicitud-row">
                <td>{{ solicitud.nombreEstudiante }}</td>
                <td>{{ solicitud.carrera }}</td>
                <td>{{ solicitud.medicamento }}</td>
                <td>{{ solicitud.cantidad }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AdminView',
  data() {
    return {
      nuevoMedicamento: {
        nombre: '',
        cantidad: 1,
        descripcion: '',
        fechaVencimiento: '',
      },
      medicamentos: [],
      solicitudes: [],
      isDarkMode: false,
      showSolicitudes: false,
    };
  },
  mounted() {
    const storedMedicamentos = JSON.parse(localStorage.getItem('medicamentos'));
    if (storedMedicamentos) {
      this.medicamentos = storedMedicamentos;
    }

    const storedSolicitudes = JSON.parse(localStorage.getItem('solicitudes'));
    if (storedSolicitudes) {
      this.solicitudes = storedSolicitudes;
    }
  },
  methods: {
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
    },
    addMedicamento() {
      const medicamento = {
        id: Date.now(),
        nombre: this.nuevoMedicamento.nombre,
        cantidad: this.nuevoMedicamento.cantidad,
        descripcion: this.nuevoMedicamento.descripcion,
        fechaVencimiento: this.nuevoMedicamento.fechaVencimiento,
      };

      // Verifica si el medicamento ya existe
      const existingMedicamento = this.medicamentos.find(m =>
        m.nombre === medicamento.nombre &&
        m.fechaVencimiento === medicamento.fechaVencimiento
      );

      if (existingMedicamento) {
        // Si existe, suma la cantidad
        existingMedicamento.cantidad += medicamento.cantidad;
      } else {
        // Si no existe, lo agrega a la lista
        this.medicamentos.push(medicamento);
      }

      // Guarda los medicamentos en localStorage
      localStorage.setItem('medicamentos', JSON.stringify(this.medicamentos));

      // Resetea el formulario
      this.nuevoMedicamento.nombre = '';
      this.nuevoMedicamento.cantidad = 1;
      this.nuevoMedicamento.descripcion = '';
      this.nuevoMedicamento.fechaVencimiento = '';
    },
    removeMedicamento(id) {
      this.medicamentos = this.medicamentos.filter(m => m.id !== id);
      localStorage.setItem('medicamentos', JSON.stringify(this.medicamentos));
    },
    toggleSolicitudes() {
      this.showSolicitudes = !this.showSolicitudes;
    },
  },
};
</script>

<style src="@/assets/viewstyles/AdminView.css" scoped></style>
