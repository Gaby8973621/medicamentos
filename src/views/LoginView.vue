<template>
  <div class="login-container">
    <!-- Video de fondo -->
    <video autoplay muted loop class="background-video">
      <source src="https://videos.pexels.com/video-files/6824180/6824180-uhd_2560_1440_30fps.mp4" type="video/mp4" />
      Tu navegador no soporta video.
    </video>

    <div class="login-card">
      <!-- Texto de la universidad -->
      <h2 class="university-name">Universidad Luterana Salvadoreña</h2>
      
      <!-- Logo de la universidad -->
      <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTfjF0hu2scy4Bo_bHN4rrNLRiKPHwuCw7jKQ&s" alt="Logo Universidad" class="university-logo" />
      
      <h1>Iniciar Sesión</h1>
      <p class="inspirational-quote">"Tu salud es lo primero: ¡siempre listo para cuidar lo que importa!".</p>
      <form @submit.prevent="login">
        <div>
          <label for="username">Usuario:</label>
          <input type="text" v-model="username" required />
        </div>
        <div>
          <label for="password">Contraseña:</label>
          <input type="password" v-model="password" required />
        </div>
        <button type="submit">Entrar</button>
      </form>
      <p class="register-prompt">¿No tienes cuenta? <router-link to="/register" class="register-link">Regístrate aquí</router-link></p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginView',
  data() {
    return {
      username: '',
      password: ''
    };
  },
  methods: {
    login() {
      // Lógica de inicio de sesión aquí
      console.log('Iniciando sesión con', this.username);

      // Aquí puedes agregar la lógica para autenticar al usuario y almacenar el estado
      localStorage.setItem('isAuthenticated', 'true'); // Solo para fines de demostración
      
      // Redirigir al usuario a la página de administración
      this.$router.push('/admin'); // Cambiar esto a la ruta de tu página de administración
    }
  }
};
</script>

<style src="@/assets/viewstyles/LoginView.css" scoped></style>
