<template>
    <ion-page>
      <ion-header>
        <ion-toolbar color="primary">
          <ion-title>Login</ion-title>
        </ion-toolbar>
      </ion-header>
  
      <ion-content>
        <div class="ion-padding">
          <ion-card>
            <ion-card-header>
              <ion-title>Connection</ion-title>
            </ion-card-header>
  
            <ion-card-content>
              <ion-input v-model="email" type="email" placeholder="Email" clear-input></ion-input>
              <ion-input v-model="password" type="password" placeholder="Password" clear-input></ion-input>
              <ion-button expand="block" @click="login">Login</ion-button>
              <ion-button expand="block" color="secondary" href="/register">Sign up</ion-button>
            </ion-card-content>
          </ion-card>
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
  
  export default defineComponent({
    setup() {
      const email = ref('');
      const password = ref('');
  
      
      const login = async () => {
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
      console.log('Login successful', data);
      // Redirect to geolocation page
      router.push('/geolocation');
    } else {
      console.error('Login failed');
      alert('Invalid email or password');
    }
  } catch (error) {
    console.error('Error occurred during login:', error);
  }
};

  
      return {
        email,
        password,
        login
      };
    }
  });
  </script>
  
  <style scoped>
  .ion-padding {
    padding: 16px;
  }
  </style>
  