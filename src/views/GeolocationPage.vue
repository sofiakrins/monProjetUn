<template>
    <ion-page>
      <ion-header>
        <ion-toolbar color="primary">
          <ion-title>Geolocation</ion-title>
          <ion-buttons slot="end">
            <ion-button @click="logout">Logout</ion-button>
          </ion-buttons>
        </ion-toolbar>
      </ion-header>
  
      <ion-content>
        <div class="ion-padding">
          <ion-button expand="block" @click="getCurrentLocation">Get Current Location</ion-button>
  
          <div v-if="location">
            <h2>Current Location:</h2>
            <p>Latitude: {{ location.latitude }}</p>
            <p>Longitude: {{ location.longitude }}</p>
            <ion-button expand="block" @click="addLocation">Add Location</ion-button>
          </div>
  
          <h2>Saved Locations:</h2>
          <ion-list>
            <ion-item v-for="(loc, index) in savedLocations" :key="index">
              {{ loc.latitude }}, {{ loc.longitude }}
            </ion-item>
          </ion-list>
  
          <ion-button expand="block" color="danger" @click="clearLocations">Clear All Locations</ion-button>
        </div>
      </ion-content>
  
      <ion-footer>
        <ion-toolbar>
          <ion-title>Footer</ion-title>
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
      const router = useRouter();
  
      const getCurrentLocation = async () => {
        const coordinates = await Geolocation.getCurrentPosition();
        location.value = {
          latitude: coordinates.coords.latitude,
          longitude: coordinates.coords.longitude,
        };
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
        // Handle logout and redirect to login page
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
  