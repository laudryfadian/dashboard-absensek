<template>
  <v-app id="login" class="primary">
    <v-main>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4 lg4>
            <v-card class="elevation-5 pa-3" color="#770000cc">
              <v-card-text>
                <div class="layout column align-center">
                  <img
                    src="../static/v.png"
                    alt="Vans"
                    width="240"
                    height="120"
                  />
                  <h1 class="flex my-4 white--text">Absen Sek Dashboard</h1>
                </div>
                <v-form>
                  <v-text-field
                    dark
                    color="white"
                    append-icon="mdi-account-circle"
                    name="email"
                    label="Email"
                    hide-details="true"
                    type="text"
                    v-model="model.email"
                    aria-autocomplete="off"
                  ></v-text-field>
                  <v-text-field
                    dark
                    color="white"
                    :append-icon="showPassword ? 'mdi-lock-open' : 'mdi-lock'"
                    @click:append="showPassword = !showPassword"
                    name="password"
                    label="Password"
                    hide-details="true"
                    id="password"
                    :type="showPassword ? 'text' : 'password'"
                    v-model="model.password"
                    aria-autocomplete="off"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn block color="primary" @click="login" :loading="loading"
                  >Login</v-btn
                >
              </v-card-actions>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn block color="success" @click="register" :loading="loading"
                  >Register</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-flex>
        </v-layout>
      </v-container>
    </v-main>
    <v-snackbar v-model="snackbar" top>
      {{ textSnackbar }}

      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  layout: "empty",
  data: () => ({
    loading: false,
    snackbar: false,
    textSnackbar: "",
    model: {
      email: "",
      password: "",
    },
    showPassword: false,
  }),
  mounted() {
    this.cekAuth();
  },
  methods: {
    cekAuth() {
      if (!localStorage.getItem("id")) {
        this.$router.push("/login");
      } else {
        this.$router.push("/home");
      }
    },
    register() {
      this.$router.push("/register-company");
    },
    async login() {
      this.loading = true;
      try {
        let response = await axios.post(`http://localhost:5000/auth/logindash`, {
          email: this.model.email,
          password: this.model.password,
        });

        const data = response.data;

        if (data.statusCode != 200) {
          this.loading = false;
          this.textSnackbar = data.message;
          this.snackbar = true;

        } else {
          localStorage.setItem("id", data.id);
          localStorage.setItem("name", data.name);
          localStorage.setItem("email", data.email);
          localStorage.setItem("idCompany", data.idCompany);
          localStorage.setItem("phone", data.phone);
          setTimeout(() => {
            this.$router.push("/home");
          }, 1000);

        }
      } catch (e) {
        this.loading = false;
        this.textSnackbar = e.response.data.message;
        this.snackbar = true;
      }
    },
  },
}
</script>
<style scoped lang="css">
#login {
  height: 50%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  content: "";
  z-index: 0;
  background-color: #b90000cc;
}
</style>
