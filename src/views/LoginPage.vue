<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Login</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div class="login-wrapper ion-padding">
        <ion-card class="login-card">
          <ion-card-header>
            <ion-title>Connection</ion-title>
          </ion-card-header>

          <ion-card-content>
            <!-- Labels flottants pour email et mot de passe -->
            <ion-item class="input-item">
              <ion-label position="floating">Email</ion-label>
              <ion-input v-model="email" type="email" clear-input></ion-input>
            </ion-item>
            <ion-item class="input-item">
              <ion-label position="floating">Mot de passe</ion-label>
              <ion-input v-model="password" type="password" clear-input></ion-input>
            </ion-item>

            <!-- Bouton de connexion -->
            <ion-button class="login-button" expand="block" @click="login" :disabled="!email || !password">Se connecter</ion-button>

            <!-- Bouton d'inscription -->
            <ion-button class="signup-button" expand="block" color="secondary" href="/register">S'inscrire</ion-button>
          </ion-card-content>
        </ion-card>

        <!-- Spinner de chargement pendant la requête de connexion -->
        <ion-spinner v-if="loading"></ion-spinner>

        <!-- Message d'erreur en cas d'échec de la connexion -->
        <div v-if="loginError" class="error-message">Erreur de connexion, veuillez réessayer.</div>
      </div>
    </ion-content>

    <ion-footer>
  <ion-toolbar color="secondary">
    <ion-title style="text-align: center; font-size: 14px;">
      © Sofia Krins et Ty Mammoliti 2024
    </ion-title>
  </ion-toolbar>
</ion-footer>

  </ion-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useRouter } from 'vue-router';

export default defineComponent({
  setup() {
    const email = ref('');
    const password = ref('');
    const loginError = ref(false);
    const loading = ref(false);
    const router = useRouter();

    const login = async () => {
      loginError.value = false;
      loading.value = true;

      if (!email.value || !password.value) {
        alert('Veuillez entrer un email et un mot de passe valides.');
        loading.value = false;
        return;
      }

      try {
        const response = await fetch('https://server-1-t93s.onrender.com/api/tp/login', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            email: email.value,
            password: password.value,
          }),
        });

        if (response.ok) {
          const data = await response.json();
          console.log(data); 
          router.push('/geolocation');
        } else {
          loginError.value = true;
        }
      } catch (error) {
        console.error('Une erreur est survenue pendant la connexion :', error);
        loginError.value = true;
      } finally {
        loading.value = false;
      }
    };

    return {
      email,
      password,
      login,
      loginError,
      loading,
    };
  },
});
</script>

<style scoped>
/* Image de fond pour cette page spécifique */
ion-content {
  background: 
    linear-gradient(rgba(255, 255, 255, 0.5), rgba(255, 255, 255, 0.5)), /* Transparence blanche */
    url('/pink-city.jpg') no-repeat center center fixed !important; /* Image de fond */
  background-size: cover !important;
  width: 100vw !important; /* Assure que l'image couvre toute la largeur */
  height: 100vh !important; /* Assure que l'image couvre toute la hauteur */
  min-height: 100vh !important;
}

/* Conteneur transparent pour le contenu */
.login-wrapper {
  background-color: transparent; /* Laisse le fond transparent */
  max-width: 500px;
  margin: 0 auto;
  padding-top: 50px;
}


.login-card {
  padding: 20px;
  border-radius: 10px;
  font-size: 1.2rem;
}

.input-item {
  margin-bottom: 20px;
}

.login-button, .signup-button {
  margin-top: 20px;
  font-size: 1.2rem;
  height: 50px;
}

.error-message {
  color: red;
  text-align: center;
  margin-top: 20px;
}

/* Styles réactifs pour les écrans plus petits */
@media (max-width: 768px) {
  .login-wrapper {
    padding: 20px;
  }

  .login-card {
    font-size: 1rem;
  }

  .login-button, .signup-button {
    font-size: 1rem;
    height: 45px;
  }

}
</style>
