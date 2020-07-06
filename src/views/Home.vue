<template>
  <div class="home">

    <h1> Welcome to the homepage</h1>

    <div style="marginTop: 2rem">
      <router-link to="/register">Register</router-link>
      <br><br>
      <p>OR</p>
      <br>
      <router-link to="/login">Login</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
}
</script>
