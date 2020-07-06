<template>
  <div>
    <form @submit.prevent="onSubmit">
      <div>
        <label for="email">Email</label>
        <input type="email" v-model.trim="email" autofocus />
      </div>
      <div>
        <label for="firstname">Firstname</label>
        <input type="text" v-model.trim="firstName" />
      </div>
      <div>
        <label for="lastname">Lastname</label>
        <input type="text" v-model.trim="lastName" />
      </div>
      <div>
        <label for="password">Password</label>
        <input type="password" v-model.trim="password" />
      </div>

      <div>
        <input type="submit" value="Register" />
      </div>
    </form>
  </div>
</template>

<script>
import axios from "axios";
import dotEnv from 'dotenv'

dotEnv.config({ debug: process.env.DEBUG });
const { VUE_APP_BASE_URL, VUE_APP_API_KEY, VUE_APP_ACCEPT } = process.env;

export default {
  name: "Register",
  data() {
    return {
      email: "",
      password: "",
      firstName: "",
      lastName: "",
    };
  },
  methods: {
    async onSubmit() {
      try {
        const res = await axios.post(`${VUE_APP_BASE_URL}/rpc/register_user`,
          {
            p_email: this.email,
            p_pass: this.password,
            p_last_name: this.lastName,
            p_first_name: this.firstName,
          },
          {
            headers: {
              "x-api-key": `${VUE_APP_API_KEY}`,
              Accept: `${VUE_APP_ACCEPT}`,
            }
          }
        );
        res.data.status === 200 ? console.log("registration Successful") : console.log("User data taken");

          this.$router.push({
            name: "login",
          });
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style></style>
