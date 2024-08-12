<script setup>
</script>

<template>
  <h1 align="center">Companies App</h1>

  <div class="com-form-container">
    <div class="com-form">
      <form action="">
        <!-- Sign in Form -->
        <div>
          <input class="com-inp" type="text" placeholder="Username" required />
          <input class="com-inp" type="password" placeholder="Password" required>
          <button class="com-btn" ype="submit">Login</button>
        </div>

        <br>
        <hr>
        <!-- Sign up Form -->
        <div style="margin-top:10px">
          <p>Don't have an account? Sign up below</p>
          <input class="com-inp" type="text" placeholder="New Username" required />
          <input class="com-inp" type="password" placeholder="New Password" required>
          <input class="com-inp" type="password" placeholder="Confirm New Password" required>
          <button class="com-btn" ype="submit">Sign up</button>
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

form div {
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
  line-height: 48px;
  padding: 0 16px;
}

.com-btn:hover{
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