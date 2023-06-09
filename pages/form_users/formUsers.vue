<template>
  <v-card>
    <v-text-field class="d-none" v-model="reload"></v-text-field>
    <v-card-title class="headline grey lighten-2 grey--text text--darken-1">
      {{ mode }} User
      <v-spacer></v-spacer>
      <v-btn small icon @click="close(null)">
        <v-icon>mdi-close</v-icon>
      </v-btn>
    </v-card-title>
    <v-card-text class="pt-5 pb-0">
      <v-text-field
        v-model="model.name"
        label="Nama"
        placeholder="Nama User"
      ></v-text-field>
      <v-text-field
        v-model="model.email"
        label="Email"
        placeholder="Alamat Email"
      ></v-text-field>
      <v-text-field
        v-model="model.password"
        label="Password"
        placeholder="Password"
      ></v-text-field>
      <v-text-field
        v-model="model.phone"
        label="Nomor HP"
        placeholder="Nomor HP"
      ></v-text-field>
      <v-text-field
        v-model="model.job"
        label="Pekerjaan"
        placeholder="Nama Pekerjaan"
      ></v-text-field>
      <v-text-field
        v-model="model.superUser"
        label="super user nanti dihapus"
        placeholder="temp"
      ></v-text-field>
      <v-text-field
        v-model="model.salary"
        label="Gaji"
        placeholder="Total Gaji"
      ></v-text-field>
      <v-text-field
        v-model="model.isAbsen"
        label="is absen nanti dihapus"
        placeholder="temp"
      ></v-text-field>
      <v-text-field
        v-model="model.jobType"
        label="jobtype nanti dihapus"
        placeholder="temp"
      ></v-text-field>
      <v-text-field
        v-model="model.idCompany"
        label="idcompany nanti dihapus"
        placeholder="temp"
      ></v-text-field>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn :loading="loadingBtn" color="red lighten-1" text @click="proses"
        >Update</v-btn
      >
    </v-card-actions>
    <v-snackbar v-model="snackbar" top>
      {{ textSnackbar }}
      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false"
          >Close</v-btn
        >
      </template>
    </v-snackbar>
  </v-card>
</template>

<script>
import axios from "axios";

export default {
  name: "formUsers",
  props: [
    "id",
    "name",
    "email",
    "password",
    "phone",
    "job",
    "superUser",
    "salary",
    "isAbsen",
    "jobType",
    "idCompany",
  ],

  computed: {
    reload() {
      this.fetch();
    },
  },

  data: () => ({
    model: {
      name: "",
      email: "",
      password: "",
      phone: "",
      job: "",
      superUser: "",
      salary: "",
      isAbsen: "",
      jobType: "",
      idCompany: "",
    },
    message: "",
    mode: "",
    loadingBtn: false,
    textSnackbar: "",
    snackbar: false,
  }),

  methods: {
    close(message) {
      this.model.name = "";
      this.model.email = "";
      this.model.password = "";
      this.model.phone = "";
      this.model.job = "";
      this.model.superUser = "";
      this.model.salary = "";
      this.model.isAbsen = "";
      this.model.jobType = "";
      this.model.idCompany = "";
      this.$emit("close", message);
    },
    fetch() {
      this.mode = "Edit";
      this.model.name = this.name;
      this.model.email = this.email;
      this.model.password = this.password;
      this.model.phone = this.phone;
      this.model.job = this.job;
      this.model.superUser = this.superUser;
      this.model.salary = this.salary;
      this.model.isAbsen = this.isAbsen;
      this.model.jobType = this.jobType;
      this.model.idCompany = this.idCompany;
    },
    async proses() {
      this.loadingBtn = true;
      try {
        await axios.put("http://localhost:5000/users/" + this.id, this.model);
        this.message = "Update Address Success";
        this.close(this.message);
      } catch (e) {
        this.textSnackbar = e.response.data.message;
        this.snackbar = true;
      }
      this.loadingBtn = false;
    },
  },
};
</script>