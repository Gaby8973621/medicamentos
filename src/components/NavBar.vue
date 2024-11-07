<template>
  <header class="navbar">
    <!-- Títulos -->
    <h1 class="university-title">UNIVERSIDAD LUTERANA SALVADOREÑA</h1>
    <h1 class="navbar-title">Botiquín Universitario</h1>

    <!-- Botones visibles en la barra de navegación -->
    <nav class="navbar-links">
      <router-link to="/" class="nav-button pill">
        🏠 Home
      </router-link>

      <!-- Mostrar siempre si el usuario ha iniciado sesión -->
      <router-link v-if="isAuthenticated" to="/admin" class="nav-button pill">
        👨‍⚕️ Admin
      </router-link>
      <router-link v-if="isAuthenticated" to="/solicitud" class="nav-button pill">
        💊 Solicitud de Medicamentos
      </router-link>

      <!-- Mostrar botón de logout solo si está autenticado -->
      <button v-if="isAuthenticated" class="nav-button pill exit-button" @click="logout">
        🚪 Salir
      </button>

      <!-- Mostrar el botón de inicio de sesión si el usuario no ha iniciado sesión -->
      <router-link v-else to="/login" class="nav-button pill">
        🔑 Iniciar Sesión
      </router-link>
    </nav>

    <!-- Menú hamburguesa en la esquina derecha -->
    <div class="hamburger-menu" @click="toggleMenu">
      <span></span>
      <span></span>
      <span></span>
    </div>

    <!-- Menú desplegable animado con pastillas -->
    <nav :class="{'navbar-links-float': true, 'active': menuActive}">
      <div class="pill-container">
        <router-link to="/" class="nav-button pill">🏠 Home</router-link>
        
        <!-- Mostrar siempre las opciones si está autenticado -->
        <router-link v-if="isAuthenticated" to="/admin" class="nav-button pill">
          👨‍⚕️ Admin
        </router-link>
        <router-link v-if="isAuthenticated" to="/solicitud" class="nav-button pill">
          💊 Solicitud
        </router-link>

        <!-- Botón de cerrar sesión -->
        <button v-if="isAuthenticated" class="nav-button pill exit-button" @click="logout">
          🚪 Salir
        </button>

        <!-- Botón de inicio de sesión si no está autenticado -->
        <router-link v-else to="/login" class="nav-button pill">
          🔑 Iniciar Sesión
        </router-link>
      </div>
    </nav>

    <!-- Línea de colores al borde -->
    <div class="color-line"></div>
  </header>
</template>

<script>
export default {
  name: 'NavBar',
  data() {
    return {
      menuActive: false, // Controla si el menú está abierto o cerrado
      isAuthenticated: false, // Controla si el usuario ha iniciado sesión
    };
  },
  mounted() {
    // Verificar si el usuario está autenticado al cargar el componente
    this.checkAuthentication();
  },
  methods: {
    toggleMenu() {
      this.menuActive = !this.menuActive; // Alterna el estado del menú
    },
    logout() {
      // Lógica para cerrar la sesión
      localStorage.removeItem('isAuthenticated'); // Elimina la autenticación
      this.isAuthenticated = false; // Actualiza el estado de autenticación
      this.$router.push('/login'); // Redirigir a la página de login
    },
    checkAuthentication() {
      // Verificar si el usuario está autenticado
      this.isAuthenticated = !!localStorage.getItem('isAuthenticated');
    }
  },
  watch: {
    // Escuchar cambios en la ruta para verificar el estado de autenticación nuevamente
    '$route'() {
      this.checkAuthentication();
    }
  }
};
</script>

<style src="@/assets/componentstyles/NavBar.css"></style>


