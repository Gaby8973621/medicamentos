<template>
  <div class="home-container">
    
    <div class="welcome-message">
      <img
        src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTfjF0hu2scy4Bo_bHN4rrNLRiKPHwuCw7jKQ&s"
        alt="Logo Botiquín Universitario"
        class="logo"
      />
      <h1>Bienvenido al Botiquín Universitario</h1>
      <p>Solicita medicamentos fácilmente y mantente saludable.</p>
      <router-link to="/login">
        <button class="enter-button">Entrar</button>
      </router-link>
 
      <div class="features">
        <div class="feature">
          <router-link to="/login">
            <img src="https://img.icons8.com/ios-filled/50/4a148c/health-book.png" alt="Health Book" />
          </router-link>
        </div>

        <div class="feature">
          <router-link to="/login">
            <img src="https://img.icons8.com/ios-filled/50/4a148c/pill.png" alt="Pill" />
          </router-link>
        </div>
      </div>
    </div>
  </div>

  <!-- Redes sociales -->
  <div class="social-media">
    <h2>Redes Sociales</h2>
    <div class="social-icons">
      <a href="https://www.facebook.com/ULuterana" target="_blank" class="social-icon">
        <img src="https://img.icons8.com/ios-filled/50/4a148c/facebook.png" alt="Facebook" />
      </a>
      <a href="https://www.instagram.com/uluterana/" target="_blank" class="social-icon">
        <img src="https://img.icons8.com/ios-filled/50/4a148c/instagram-new.png" alt="Instagram" />
      </a>
      <a href="https://www.youtube.com/@uls1991" target="_blank" class="social-icon">
        <img src="https://img.icons8.com/ios-filled/50/4a148c/youtube.png" alt="YouTube" />
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HomeView',
};
</script>

<style src="@/assets/viewstyles/HomeView.css" scoped></style>
