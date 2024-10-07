<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-buttons slot="start">
          <ion-back-button></ion-back-button>
        </ion-buttons>
        <ion-title>Registration</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div class="register-wrapper ion-padding">
        <ion-card class="register-card">
          <ion-card-header>
            <ion-title>Inscription</ion-title>
          </ion-card-header>

          <ion-card-content>
            <!-- Champs de saisie avec labels flottants pour prénom, nom, email et mot de passe -->
            <ion-item class="input-item">
              <ion-label position="floating">Prénom</ion-label>
              <ion-input v-model="firstName" clear-input></ion-input>
            </ion-item>
            <ion-item class="input-item">
              <ion-label position="floating">Nom</ion-label>
              <ion-input v-model="lastName" clear-input></ion-input>
            </ion-item>
            <ion-item class="input-item">
              <ion-label position="floating">Email</ion-label>
              <ion-input v-model="email" type="email" clear-input></ion-input>
            </ion-item>
            <ion-item class="input-item">
              <ion-label position="floating">Mot de passe</ion-label>
              <ion-input v-model="password" type="password" clear-input></ion-input>
            </ion-item>

            <!-- Bouton d'inscription -->
            <ion-button class="register-button" expand="block" @click="register">
              S'inscrire
            </ion-button>
          </ion-card-content>
        </ion-card>

        <!-- Spinner de chargement pendant la requête d'inscription -->
        <ion-spinner v-if="loading"></ion-spinner>

        <!-- Message d'erreur en cas d'échec de l'inscription -->
        <div v-if="registerError" class="error-message">Échec de l'inscription, veuillez réessayer.</div>
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
    const firstName = ref('');
    const lastName = ref('');
    const email = ref('');
    const password = ref('');
    const registerError = ref(false);
    const loading = ref(false);
    const router = useRouter();

    const register = async () => {
      registerError.value = false;
      loading.value = true;

      try {
        const response = await fetch('https://server-1-t93s.onrender.com/api/tp/signup', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            firstName: firstName.value,
            lastName: lastName.value,
            email: email.value,
            password: password.value,
          }),
        });

        if (response.ok) {
          const data = await response.json();
          console.log('Utilisateur inscrit avec succès', data);
          // Rediriger vers la page de connexion
          router.push('/login');
        } else {
          registerError.value = true;
        }
      } catch (error) {
        console.error('Une erreur est survenue pendant l\'inscription :', error);
        registerError.value = true;
      } finally {
        loading.value = false;
      }
    };

    return {
      firstName,
      lastName,
      email,
      password,
      registerError,
      loading,
      register,
    };
  },
});
</script>

<style scoped>
/* Ensure spacing and larger elements for better UI */
.register-wrapper {
  max-width: 500px;
  margin: 0 auto;
  padding-top: 50px;
}

.register-card {
  padding: 20px;
  border-radius: 10px;
  font-size: 1.2rem;
}

.input-item {
  margin-bottom: 20px;
}

.register-button {
  margin-top: 20px;
  font-size: 1.2rem;
  height: 50px;
}

/* Error message style */
.error-message {
  color: red;
  text-align: center;
  margin-top: 20px;
}

/* Responsive styles for smaller screens */
@media (max-width: 768px) {
  .register-wrapper {
    padding: 20px;
  }

  .register-card {
    font-size: 1rem;
  }

  .register-button {
    font-size: 1rem;
    height: 45px;
  }
}
</style>
