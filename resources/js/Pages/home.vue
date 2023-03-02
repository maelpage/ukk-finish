<template>
  <v-app>
    <div class="d-flex row justify-center login-container">
        <div class="text-center">
            <h1 color="blue--text">Report Websites</h1>
        <v-card class="rounded-lg mt-5" width="400" elevation="2" height="340">
            <div class="text-center" style="margin-top:15px">
                <h2 class="text-center">Login</h2>
            </div>
            <v-card-text>
                <v-text-field
                    label="Username"
                    outline
                    v-model="form.username"
                ></v-text-field>
                <v-text-field
                    label="password"
                    outline
                    type="password"
                    v-model="form.password"
                ></v-text-field>
                <v-btn color="primary" @click="login" block>Login</v-btn>
                <div class="mt-5">
                    <v-btn outlined block @click="register" elevation="2">Register</v-btn>
                </div>
            </v-card-text>
        </v-card>
        </div>
    </div>
  </v-app>
</template>

<script>
import { router } from "@inertiajs/vue3";
export default {
  props: {
    errors: Object,
  },
  data() {
    return {
      form: {
        username: "",
        password: "",
      },
    };
  },
  methods: {
    login() {
      router.post("/login", this.form);
    },
    register() {
      router.get("/register");
    },
  },
};
</script>
<style>
.login-container {
    background-image: url("img/background-admin.jpg");
    background-size: cover;
    background-position: center;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
</style>
