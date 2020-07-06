<template>
  <div id="user" v-if="firstName !== ''">
    <h2>Hello, {{ firstName }} {{ lastName }}</h2>

    <p>Registered at: {{ registered }}</p>

    <button @click="onLogout">Logout</button>
  </div>
</template>

<script>
import axios from "axios";
import dotEnv from "dotenv";
dotEnv.config({ debug: process.env.DEBUG });

const { VUE_APP_BASE_URL, VUE_APP_API_KEY, VUE_APP_ACCEPT } = process.env;

export default {
  name: "User",
  data() {
    return {
      token: undefined,
      firstName: "",
      lastName: "",
      registered: "",
    };
  },
  methods: {
    async loadUser() {
      this.token = this.$route.params.token;
      try {
        const res = await axios.get(`${VUE_APP_BASE_URL}/user_home`, {
          headers: {
            "x-api-key": `${VUE_APP_API_KEY}`,
            Accept: `${VUE_APP_ACCEPT}`,
            Authorization: `Bearer ${this.token}`,
          },
        });

        this.firstName = res.data.first_name;
        this.lastName = res.data.last_name;
        this.registered = res.data.registered_datetime;
      } catch (error) {
        console.log(error.message);
      }
    },
    onLogout() {
      this.firstName = "";
      this.lastName = "";
      this.registered = "";
      this.token = "";
      this.$route.params.token = "";
      this.$router.push("/login");
    },
  },
  created() {
    this.loadUser();
  },
};
</script>


<style >
#user {
  line-height: 3;
}
</style>
