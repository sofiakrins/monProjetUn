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
        <!-- Affiche les coordonnées de longitude -->
        <ion-item>
          <ion-label>Longitude: </ion-label>
          <ion-label>{{ location ? location.longitude : 'Chargement...' }}</ion-label>
        </ion-item>
        <!-- Affiche les coordonnées de latitude -->
        <ion-item>
          <ion-label>Latitude: </ion-label>
          <ion-label>{{ location ? location.latitude : 'Chargement...' }}</ion-label>
        </ion-item>

        <!-- Bouton pour obtenir l'emplacement actuel -->
        <ion-button expand="block" @click="getCurrentLocation">Obtenir la position actuelle</ion-button>

        <!-- Bouton pour ajouter l'emplacement actuel -->
        <ion-button expand="block" @click="addLocation" v-if="location">Ajouter Emplacement</ion-button>

        <h2>Emplacements Enregistrés:</h2>
        <!-- Liste des emplacements sauvegardés -->
        <ion-list>
          <ion-item v-for="(loc, index) in savedLocations" :key="index">
            {{ loc.latitude }}, {{ loc.longitude }}
          </ion-item>
        </ion-list>

        <!-- Bouton pour effacer tous les emplacements -->
        <ion-button expand="block" color="danger" @click="clearLocations">Effacer Tout</ion-button>
      </div>
    </ion-content>

    <ion-footer>
      <ion-toolbar>
        <ion-title>Pied de page</ion-title>
      </ion-toolbar>
    </ion-footer>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import { Geolocation } from '@capacitor/geolocation';
import { useRouter } from 'vue-router';

export default defineComponent({
  setup() {
    // Variables pour stocker l'emplacement actuel et les emplacements enregistrés
    const location = ref<{ latitude: number, longitude: number } | null>(null);
    const savedLocations = ref<Array<{ latitude: number, longitude: number }>>([]);
    const router = useRouter();

    // Fonction pour obtenir l'emplacement actuel de l'utilisateur
    const getCurrentLocation = async () => {
      try {
        const coordinates = await Geolocation.getCurrentPosition();
        location.value = {
          latitude: coordinates.coords.latitude,
          longitude: coordinates.coords.longitude,
        };
      } catch (error) {
        // Gérer l'erreur en affichant un message dans la console et une alerte à l'utilisateur
        console.error("Erreur lors de la récupération de la position: ", error);
        alert("Impossible de récupérer la position. Vérifiez votre GPS ou vos permissions.");
      }
    };

    // Utiliser mounted() pour exécuter getCurrentLocation quand le composant est monté
    onMounted(() => {
      getCurrentLocation();
    });

    // Ajouter l'emplacement actuel à la liste des emplacements sauvegardés
    const addLocation = () => {
      if (location.value) {
        savedLocations.value.push({
          latitude: location.value.latitude,
          longitude: location.value.longitude,
        });
      }
    };

    // Effacer tous les emplacements enregistrés
    const clearLocations = () => {
      savedLocations.value = [];
    };

    // Fonction de déconnexion et redirection vers la page de connexion
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
    };
  }
});
</script>

<style scoped>
.ion-padding {
  padding: 16px;
}
</style>
