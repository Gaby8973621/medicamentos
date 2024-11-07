<template>
  <div class="solicitud-container">
    <h2 class="title">Solicitar Medicamentos</h2>
    <form @submit.prevent="solicitarMedicamento" class="solicitud-form">
      <div class="form-group">
        <label for="nombreEstudiante">Nombre del Estudiante:</label>
        <input
          type="text"
          v-model="solicitud.nombreEstudiante"
          id="nombreEstudiante"
          required
          class="form-input"
        />
      </div>
      <div class="form-group">
        <label for="carrera">Carrera:</label>
        <select
          v-model="solicitud.carrera"
          id="carrera"
          required
          class="form-input"
        >
          <option disabled value="">Selecciona una carrera</option>
          <option value="Licenciatura en Computación">Licenciatura en Computación</option>
          <option value="Licenciatura en Trabajo Social">Licenciatura en Trabajo Social</option>
          <option value="Licenciatura en Idioma">Licenciatura en Idioma</option>
          <option value="Licenciatura en Psicología">Licenciatura en Psicología</option>
        </select>
      </div>
      <div class="form-group">
        <label for="medicamento">Medicamento:</label>
        <select
          v-model="solicitud.medicamento"
          id="medicamento"
          required
          class="form-input"
        >
          <option disabled value="">Selecciona un medicamento</option>
          <option v-for="medicamento in medicamentos" :key="medicamento.id" :value="medicamento.nombre">
            {{ medicamento.nombre }}
          </option>
        </select>
      </div>
      <div class="form-group">
        <label for="cantidad">Cantidad:</label>
        <input
          type="number"
          v-model="solicitud.cantidad"
          id="cantidad"
          min="1"
          required
          class="form-input"
        />
      </div>
      <div class="form-group">
        <label for="fecha">Fecha de Solicitud:</label>
        <input
          type="date"
          v-model="solicitud.fecha"
          id="fecha"
          required
          class="form-input"
        />
      </div>
      <button type="submit" class="submit-button">Solicitar Medicamento</button>
      <p v-if="mensajeError" class="error-message">{{ mensajeError }}</p>
    </form>
    
    <!-- Botón para generar PDF -->
    <button @click="generarPDF" class="submit-button-pdf">Generar PDF</button>

    <div class="solicitudes-card">
      <h3>Solicitudes Realizadas</h3>
      <ul class="solicitudes-list">
        <li v-for="(solicitud, index) in solicitudes" :key="index">
          {{ solicitud.nombreEstudiante }} ({{ solicitud.carrera }}) - Fecha: {{ solicitud.fecha }}: {{ solicitud.medicamento }} - Cantidad: {{ solicitud.cantidad }}
          <button @click="eliminarSolicitud(index)">Eliminar</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import jsPDF from 'jspdf';

export default {
  name: 'SolicitudMedicamento',
  data() {
    return {
      solicitud: {
        nombreEstudiante: '',
        carrera: '',
        medicamento: '',
        cantidad: 1,
        fecha: '', 
      },
      solicitudes: [],
      medicamentos: [],
      mensajeError: '',
    };
  },
  mounted() {
    const storedSolicitudes = JSON.parse(localStorage.getItem('solicitudes'));
    if (storedSolicitudes) {
      this.solicitudes = storedSolicitudes;
    }

    const storedMedicamentos = JSON.parse(localStorage.getItem('medicamentos'));
    if (storedMedicamentos) {
      this.medicamentos = storedMedicamentos;
    }
  },
  methods: {
    solicitarMedicamento() {
      const medicamentoSeleccionado = this.medicamentos.find(m => m.nombre === this.solicitud.medicamento);
      if (medicamentoSeleccionado && medicamentoSeleccionado.cantidad >= this.solicitud.cantidad) {
        this.solicitudes.push({ ...this.solicitud });
        localStorage.setItem('solicitudes', JSON.stringify(this.solicitudes));
        this.mensajeError = '';
        this.solicitud = {
          nombreEstudiante: '',
          carrera: '',
          medicamento: '',
          cantidad: 1,
          fecha: '', 
        };
      } else {
        this.mensajeError = 'No hay suficiente cantidad disponible.';
      }
    },
    eliminarSolicitud(index) {
      this.solicitudes.splice(index, 1);
      localStorage.setItem('solicitudes', JSON.stringify(this.solicitudes));
    },
    generarPDF() {
  const doc = new jsPDF();

  doc.setFontSize(18);
  doc.text('Solicitudes de Medicamentos', 10, 10);

  let yPos = 20;
  const padding = 5;
  const lineHeight = 10;
  const boxWidth = 180; // Ancho del cuadro
  const boxHeight = 45; // Alto del cuadro para cada solicitud

  this.solicitudes.forEach((solicitud, index) => {
    const text = `${index + 1}. Estudiante: ${solicitud.nombreEstudiante}`;
    const textCarrera = `Carrera: ${solicitud.carrera}`;
    const textMedicamento = `Medicamento: ${solicitud.medicamento}`;
    const textCantidad = `Cantidad: ${solicitud.cantidad}`;
    const textFecha = `Fecha: ${solicitud.fecha}`;

    // Estilos del cuadro
    const borderColor = [76, 17, 122]; // Color del borde (púrpura)
    const fillColor = [238, 234, 247]; // Color de fondo (lila claro)
    
    // Dibuja el rectángulo alrededor de la solicitud con bordes estilizados
    doc.setDrawColor(...borderColor); // Color del borde
    doc.setFillColor(...fillColor); // Color de fondo
    doc.rect(10, yPos - padding, boxWidth, boxHeight, 'FD'); // 'FD' para rellenar y dibujar el borde
    
    // Escribe el contenido dentro del rectángulo
    doc.setFontSize(12);
    doc.text(text, 15, yPos);
    doc.text(textCarrera, 15, yPos + lineHeight);
    doc.text(textMedicamento, 15, yPos + lineHeight * 2);
    doc.text(textCantidad, 15, yPos + lineHeight * 3);
    doc.text(textFecha, 15, yPos + lineHeight * 4);
    
    // Aumenta la posición Y para la siguiente solicitud
    yPos += boxHeight + padding;
  });

  doc.save('solicitudes.pdf');
},

  },
};
</script>


<style src="@/assets/viewstyles/SolicitudMedicamento.css" scoped></style>
