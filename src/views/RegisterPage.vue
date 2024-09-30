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
        <div class="ion-padding">
          <ion-card>
            <ion-card-header>
              <ion-title>Register</ion-title>
            </ion-card-header>
  
            <ion-card-content>
              <!-- First Name Input -->
              <ion-input v-model="firstName" placeholder="First Name" clear-input></ion-input>
  
              <!-- Last Name Input -->
              <ion-input v-model="lastName" placeholder="Last Name" clear-input></ion-input>
  
              <!-- Email Input -->
              <ion-input v-model="email" type="email" placeholder="Email" clear-input></ion-input>
  
              <!-- Password Input -->
              <ion-input v-model="password" type="password" placeholder="Password" clear-input></ion-input>
  
              <!-- Sign Up Button -->
              <ion-button expand="block" @click="register">Sign up</ion-button>
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
      const firstName = ref('');
      const lastName = ref('');
      const email = ref('');
      const password = ref('');
  
      const register = async () => {
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
      console.log('User registered successfully', data);
      // Redirect to login
    } else {
      console.error('Registration failed');
      alert('Registration failed, please try again.');
    }
  } catch (error) {
    console.error('Error occurred during registration:', error);
  }
};
  
      return {
        firstName,
        lastName,
        email,
        password,
        register,
      };
    }
  });
  </script>

<style scoped>
.ion-padding {
  padding: 16px;
}
</style>

  
  