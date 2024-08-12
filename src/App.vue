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
      alert('Login successful!');
    }

  } catch (err) {
    alert("Username or password incorrect")
  }
}
</script>

<template>
  <h1 align="center">Companies App</h1>

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

<style scoped>
.com-form-container {
  display: flex;
  justify-content: center;
}

.com-form {
  text-align: center;
  /* width: 300px; */
  background-color: #fff;
  border-radius: 6px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, .1), 0 8px 16px rgba(0, 0, 0, .1);
  padding: 15px;
}

.com-form div {
  display: flex;
  flex-direction: column;
}

input {
  margin: 3px;
}

.com-btn {
  margin: 3px;
  background: #252122;
  color: #fff;
  border-radius: 5px;
  cursor: pointer;
  border: 0;

  font-size: 20px;
  font-weight: 600;
  line-height: 48px;
  padding: 0 16px;
}

.com-btn:hover {
  background-color: black;
  transition: 0.5s;
}

.com-inp {
  padding: 14px 16px;
  font-size: 17px;
  width: 330px;
  border-radius: 6px;
  border: 1px solid #dddfe2;
  color: #1d2129;
  margin-top: 6px;
  margin-bottom: 6px;
}
</style>