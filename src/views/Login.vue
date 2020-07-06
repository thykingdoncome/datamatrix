<template>
  <div>
    <form @submit.prevent="onLogin">
      <div>
        <label for="email">Email</label>
        <input type="email" v-model.trim="email" autofocus />
      </div>

      <div>
        <label for="password">Password</label>
        <input type="password" v-model.trim="password" />
      </div>

      <div>
        <input type="submit" value="Login" />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
import dotEnv from 'dotenv'
dotEnv.config({ debug: process.env.DEBUG })

const {
    VUE_APP_BASE_URL,
     VUE_APP_API_KEY,
     VUE_APP_ACCEPT
     } = process.env;

export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      token: undefined
    };
  },
  methods: {
    async onLogin() {
      try {
        const res = await axios.post(
          `${VUE_APP_BASE_URL}/rpc/login`,
          {
            email: this.email,
            pass: this.password,
          },
          {
            headers: {
              "x-api-key": `${VUE_APP_API_KEY}`,
              Accept: `${VUE_APP_ACCEPT}`,
            },
          }
        );
      
        this.token = res.data.token;
         this.$router.push({
            name: "user_home",
            params: { token: this.token },
          });
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style></style>
