<template>
  <v-app id="login" class="primary">
    <v-main>
      <v-container fluid fill-height>
        <v-layout align-center justify-center>
          <v-flex xs12 sm8 md4 lg4>
            <v-card class="elevation-5 pa-3" color="#770000cc">
              <v-card-text>
                <!-- <div class="layout column align-center">
                  <img
                    src="../static/v.png"
                    alt="Vans"
                    width="240"
                    height="120"
                  />
                  <h1 class="flex my-4 white--text">Absen Sek Dashboard</h1>
                </div> -->
                <v-form>
                  <v-text-field
                    dark
                    color="white"
                    append-icon="mdi-account-circle"
                    name="nama"
                    label="Nama"
                    hide-details="true"
                    type="text"
                    v-model="model.name"
                    aria-autocomplete="off"
                  ></v-text-field>
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
                  <v-text-field
                    dark
                    color="white"
                    append-icon="mdi-account-circle"
                    name="nohp"
                    label="Nomor hp"
                    hide-details="true"
                    type="number"
                    v-model="model.phone"
                    aria-autocomplete="off"
                  ></v-text-field>
                  <v-text-field
                    dark
                    color="white"
                    append-icon="mdi-account-circle"
                    name="job"
                    label="Pekerjaan"
                    hide-details="true"
                    type="text"
                    v-model="model.job"
                    aria-autocomplete="off"
                  ></v-text-field>
                  <v-text-field
                    dark
                    color="white"
                    append-icon="mdi-account-circle"
                    name="gaji"
                    label="Gaji"
                    hide-details="true"
                    type="number"
                    v-model="model.salary"
                    aria-autocomplete="off"
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <!-- <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn block color="primary" @click="login" :loading="loading"
                  >Login</v-btn
                >
              </v-card-actions> -->
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  block
                  color="success"
                  @click="register"
                  :loading="loading"
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
    <div>
      <!-- <v-btn @click="showDialog = true">Open Dialog</v-btn> -->
      <v-dialog v-model="showDialog" max-width="300px">
        <v-card>
          <v-card-title>Login Berhasil</v-card-title>
          <v-card-text>
            <p>Silahkan login untuk masuk dashboard</p>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" @click="toLogin()">Login</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
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
    showDialog: false,
    model: {
      name: "",
      email: "",
      password: "",
      phone: "",
      job: "",
      salary: "",
    },
    showPassword: false,
  }),
  methods: {
    async register() {
      try {
        if (
          Object.values(this.model).some(
            (value) => value === null || value === ""
          )
        ) {
          this.loading = false;
          this.textSnackbar = "form harus diisi semua";
          this.snackbar = true;
        } else {
          const req = {
            name: this.model.name,
            email: this.model.email,
            password: this.model.password,
            phone: this.model.phone.toString(),
            job: this.model.job,
            superUser: false,
            salary: this.model.salary,
            idCompany: localStorage.getItem("idCompany"),
            isAbsen: false,
            jobType: "admin",
          };

          const response = await axios.post("http://localhost:5000/users", req);

          const data = response.data;

          if (data.statusCode != 200) {
            this.loading = false;
            this.textSnackbar = data.message;
            this.snackbar = true;
          } else {
            this.showDialog = true;

          }
        }
      } catch (e) {
        this.loading = false;
        this.textSnackbar = e.response.data.message;
        this.snackbar = true;
      }
    },
    toLogin() {
      this.$router.push("/login");
    }
  },
};
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
