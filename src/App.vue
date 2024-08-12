<script setup>
import { ref } from 'vue';
import axios from "axios"

//refs for login fields
const username = ref('')
const loginpassword = ref('')

//refs for signup fields
const password = ref('');
const confirmPassword = ref('');
const error = ref('');

// 1. Signup Function
function handleSubmitSignUpForm() {
  if (password.value !== confirmPassword.value) {
    error.value = 'Passwords do not match!';
  }
  else {
    // Create account after confirming user duplicate not in database
  }
}

// 2. Login Function
async function handleLogin() {
  try {
    const response = await axios.post('https://companiesflaskapi.onrender.com/oauth/token', {
      username: username.value,
      password: loginpassword.value,
    });

    if (response.data.access_token) {
      //store the token
      localStorage.setItem('access_token', response.data.access_token);
      // alert('Login successful!');
      console.log("✅ Login successful")
      document.querySelector('.com-nav').style.display = 'flex';
      document.querySelector('.com-headline').style.display = 'none';
      document.querySelector('.com-form').style.display = 'none';
    }

  } catch (err) {
    alert("Username or password incorrect")
  }
}

// Logout Function
function handleLogout() {
  console.log("✅ Logout successful")
  document.querySelector('.com-nav').style.display = 'none';
  document.querySelector('.com-headline').style.display = 'block';
  document.querySelector('.com-form').style.display = 'block';
}
</script>

<template>

  <nav class="com-nav">
    <div>
      <h1>Companies App</h1>
    </div>
    <div class="com-nav-right">
      <p>Welcome, {{ username }}</p>
      <button class="logout-btn" @click="handleLogout">Logout</button>
    </div>
  </nav>


  <h1 class="com-headline">Companies App</h1>

  <div class="com-form-container">
    <div class="com-form">

      <!-- Sign in Form -->
      <form @submit.prevent="handleLogin">
        <div>
          <input class="com-inp" type="text" v-model="username" placeholder="Username" required />
          <input class="com-inp" type="password" v-model="loginpassword" placeholder="Password" required>
          <button class="com-btn" type="submit">Login</button>
        </div>
      </form>

      <br>
      <hr>
      <!-- Sign up Form -->
      <form @submit.prevent="handleSubmitSignUpForm">
        <div style="margin-top:10px">
          <p>Don't have an account? Sign up below</p>
          <input class="com-inp" type="text" placeholder="New Username" required />
          <input class="com-inp" type="password" v-model="password" placeholder="New Password" required>
          <input class="com-inp" type="password" v-model="confirmPassword" placeholder="Confirm New Password" required>
          <p v-if="error" style="color: red;">{{ error }}</p>
          <button class="com-btn" type="submit">Sign up</button>
        </div>
      </form>
    </div>

  </div>
</template>