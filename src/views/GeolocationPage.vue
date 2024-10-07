<template>
  <ion-page>
    <ion-header>
      <ion-toolbar color="primary">
        <ion-title>Géolocalisation</ion-title>
        <ion-buttons slot="end">
          <!-- Bouton pour se déconnecter -->
          <ion-button @click="logout">Déconnecter</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <div class="ion-padding">
        <!-- Bouton pour obtenir l'emplacement actuel -->
        <ion-button expand="block" @click="getCurrentLocation">Obtenir la position actuelle</ion-button>

        <!-- Affichage du spinner pendant la récupération de l'emplacement actuel -->
        <ion-spinner v-if="loading"></ion-spinner>

        <!-- Affichage de la position lorsque récupérée -->
        <div v-if="location && !loading">
          <ion-card class="geolocation-card">
            <ion-card-header>
              <ion-title>Votre Position Actuelle</ion-title>
            </ion-card-header>
            <ion-card-content>
              <ion-item>
                <ion-label>Latitude: </ion-label>
                <ion-label>{{ location.latitude }}</ion-label>
              </ion-item>
              <ion-item>
                <ion-label>Longitude: </ion-label>
                <ion-label>{{ location.longitude }}</ion-label>
              </ion-item>
            </ion-card-content>
          </ion-card>

          <!-- Bouton pour ajouter l'emplacement actuel -->
          <ion-button expand="block" @click="addLocation">Ajouter Emplacement</ion-button>
        </div>

        <h2>Emplacements Enregistrés:</h2>
        <!-- Liste des emplacements sauvegardés -->
        <ion-list>
          <ion-item v-for="(loc, index) in savedLocations" :key="index">
            {{ loc.latitude }}, {{ loc.longitude }}
          </ion-item>
        </ion-list>

        <!-- Bouton pour effacer tous les emplacements -->
        <ion-button expand="block" color="danger" @click="clearLocations">
          Effacer Tout
        </ion-button>
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
import { Geolocation } from '@capacitor/geolocation';
import { useRouter } from 'vue-router';

export default defineComponent({
  setup() {
    const location = ref<{ latitude: number, longitude: number } | null>(null);
    const savedLocations = ref<Array<{ latitude: number, longitude: number }>>([]);
    const loading = ref(false); // Ajout de l'état de chargement
    const router = useRouter();

    const getCurrentLocation = async () => {
      loading.value = true; // Commence le chargement
      try {
        const coordinates = await Geolocation.getCurrentPosition();
        location.value = {
          latitude: coordinates.coords.latitude,
          longitude: coordinates.coords.longitude,
        };
      } catch (error) {
        console.error('Erreur lors de la récupération de la position :', error);
        alert('Impossible de récupérer la position. Vérifiez votre GPS ou vos permissions.');
      } finally {
        loading.value = false; // Arrête le chargement
      }
    };

    const addLocation = () => {
      if (location.value) {
        savedLocations.value.push({
          latitude: location.value.latitude,
          longitude: location.value.longitude,
        });
      }
    };

    const clearLocations = () => {
      savedLocations.value = [];
    };

    const logout = () => {
      router.push('/login');
    };

    return {
      location,
      savedLocations,
      getCurrentLocation,
      addLocation,
      clearLocations,
      logout,
      loading, // Ajout du retour de l'état de chargement
    };
  }
});
</script>

<style scoped>
/* Carte de géolocalisation */
.geolocation-card {
  margin-top: 20px;
}

/* Padding de base */
.ion-padding {
  padding: 16px;
}
</style>
