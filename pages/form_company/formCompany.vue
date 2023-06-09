<template>
  <v-card>
    <v-text-field class="d-none" v-model="reload"></v-text-field>
    <v-card-title class="headline grey lighten-2 grey--text text--darken-1">
      {{ mode }} Company
      <v-spacer></v-spacer>
      <v-btn small icon @click="close(null)">
        <v-icon>mdi-close</v-icon>
      </v-btn> </v-card-title
    ><v-card-text class="pt-5 pb-0">
      <v-text-field
        v-model="model.name"
        label="Perusahaan"
        placeholder="Name Perusahaan"
      ></v-text-field>
      <v-text-field
        v-model="model.address"
        label="Alamat"
        placeholder="Alamat Lengkap"
      ></v-text-field>
    </v-card-text>
    <v-card-actions
      ><v-spacer></v-spacer>
      <v-btn :loading="loadingBtn" color="red lighten-1" text @click="proses"
        >Update</v-btn
      ></v-card-actions
    >
    <v-snackbar v-model="snackbar" top>
      {{ textSnackbar }}

      <template v-slot:action="{ attrs }">
        <v-btn color="red" text v-bind="attrs" @click="snackbar = false">
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-card>
</template>
<script>
import axios from "axios";
export default {
  name: "formCompany",
  props: ["id", "name", "address"],
  computed: {
    reload() {
      this.fetch();
    },
  },
  data: () => ({
    model: {
      name: "",
      address: "",
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
      this.model.address = "";
      this.$emit("close", message);
    },
    fetch() {
      this.mode = "Edit";
      this.model.name = this.name;
      this.model.address = this.address;
    },
    async proses() {
      this.loadingBtn = true;
      try {
        await axios.put("http://localhost:5000/companys/" + this.id, {
          name: this.model.name,
          address: this.model.address,
        });
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
