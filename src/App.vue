<script setup>
import { ref, onMounted } from 'vue';
import axios from "axios"

// Import Components
import CompanySelector from './components/CompanySelector.vue';

const isloggedin = ref(0);

onMounted(() => {
  const savedLoginState = localStorage.getItem('isloggedin');
  const savedUser = localStorage.getItem("username")

  if (savedLoginState) {
    isloggedin.value = parseInt(savedLoginState);
    username.value = savedUser
  }
  loggedInChecker();
});

function loggedInChecker() {
  if (isloggedin.value === 0) {
    document.querySelector('.com-nav').style.display = 'none';
    document.querySelector('.com-headline').style.display = 'block';
    document.querySelector('.com-form').style.display = 'block';
    document.querySelector('.com-list').style.display = 'none';
  } else if (isloggedin.value === 1) {
    document.querySelector('.com-nav').style.display = 'flex';
    document.querySelector('.com-headline').style.display = 'none';
    document.querySelector('.com-form').style.display = 'none';
    document.querySelector('.com-list').style.display = 'block';
  }
}

//refs for login fields
const username = ref('')
const loginpassword = ref('')

//refs for signup fields
const newusername = ref('')
const password = ref('');
const confirmPassword = ref('');
const error = ref('');

// 1. Signup Function
async function handleSubmitSignUpForm() {
  try {
    //A. If passwords dont match
    if (password.value !== confirmPassword.value) {
      error.value = 'Passwords do not match!';
      return
    }
    //B. If passwords match then create new user
    else {
      const response = await axios.post('http://127.0.0.1:5000/signup', {
        username: newusername.value,
        password: password.value,
      });

      //When sign up successful, go to dashboard
      if (response.data.message) {
        console.log("✅ Sign-up successful");
        localStorage.setItem('isloggedin', 1);
        username.value = newusername.value
        localStorage.setItem('username', username.value);
        document.querySelector('.com-nav').style.display = 'flex';
        document.querySelector('.com-headline').style.display = 'none';
        document.querySelector('.com-form').style.display = 'none';
        document.querySelector('.com-list').style.display = 'block';
        alert("Sign up successful")
      }

    }

  } catch (err) {
    if (err.response) {
      if (err.response.status === 400) {
        alert("Username already exists")
      }
    }
    else {
      alert("Sign-up failed: " + err.message);
    }

  }

}

// 2. Login Function
async function handleLogin() {
  try {
    const response = await axios.post('http://localhost:5000/oauth/token', {
      username: username.value,
      password: loginpassword.value,
    });

    if (response.data.access_token) {
      //store the token
      localStorage.setItem('access_token', response.data.access_token);
      // alert('Login successful!');
      console.log("✅ Login successful")
      localStorage.setItem('isloggedin', 1);
      localStorage.setItem('username', username.value);
      document.querySelector('.com-nav').style.display = 'flex';
      document.querySelector('.com-headline').style.display = 'none';
      document.querySelector('.com-form').style.display = 'none';
      document.querySelector('.com-list').style.display = 'block';
    }

  } catch (err) {

    if (err.response) {
      if (err.response.status === 401) {
        alert("Username or password incorrect")
      }
    }

    console.log(err.message)
  }
}

// Logout Function
function handleLogout() {
  console.log("✅ Logout successful")
  localStorage.removeItem('isloggedin');
  localStorage.removeItem('username');
  document.querySelector('.com-nav').style.display = 'none';
  document.querySelector('.com-headline').style.display = 'block';
  document.querySelector('.com-form').style.display = 'block';
  document.querySelector('.com-list').style.display = 'none';
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
          <input class="com-inp" type="text" v-model="newusername" placeholder="New Username" required />
          <input class="com-inp" type="password" v-model="password" placeholder="New Password" required>
          <input class="com-inp" type="password" v-model="confirmPassword" placeholder="Confirm New Password" required>
          <p v-if="error" style="color: red;">{{ error }}</p>
          <button class="com-btn" type="submit">Sign up</button>
        </div>
      </form>
    </div>

  </div>

  <div class="com-list">
    <CompanySelector />
  </div>
</template>