<template>
  <div class="video-background">
    <!-- Video de fondo -->
    <video autoplay muted loop class="background-video">
      <source src="https://videos.pexels.com/video-files/6824185/6824185-uhd_2560_1440_30fps.mp4" type="video/mp4" />
      Tu navegador no soporta video de fondo.
    </video>

    <!-- Contenedor del formulario de registro -->
    <div class="register-container">
      <div class="register-card">
        <!-- Logo -->
        <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTfjF0hu2scy4Bo_bHN4rrNLRiKPHwuCw7jKQ&s" alt="Logo" class="logo" />
        <h1>Registrar Cuenta</h1>
        <p class="inspirational-quote">"Cada nuevo comienzo proviene de algún otro comienzo que tiene un final."</p>
        <form @submit.prevent="register">
          <div>
            <label for="username">Usuario:</label>
            <input type="text" v-model="username" required />
          </div>
          <div>
            <label for="password">Contraseña:</label>
            <input type="password" v-model="password" required />
          </div>
          <div>
            <label for="confirmPassword">Confirmar Contraseña:</label>
            <input type="password" v-model="confirmPassword" required />
          </div>
          <button type="submit">Registrar</button>
        </form>
        <p class="login-prompt">¿Ya tienes cuenta? <router-link to="/" class="login-link">Inicia sesión aquí</router-link></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RegisterView',
  data() {
    return {
      username: '',
      password: '',
      confirmPassword: ''
    };
  },
  methods: {
    register() {
      if (this.password === this.confirmPassword) {
        console.log('Registrando usuario:', this.username);
        localStorage.setItem('isAuthenticated', 'true');
        this.$router.push('/'); // Redirigir al home después del registro
      } else {
        alert("Las contraseñas no coinciden.");
      }
    }
  }
};
</script>

<style src="@/assets/viewstyles/RegisterView.css" scoped></style>

