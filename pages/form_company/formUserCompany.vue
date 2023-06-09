<template>
  <v-card>
    <v-text-field class="d-none" v-model="reload"></v-text-field>
    <v-card-title class="headline grey lighten-2 grey--text text--darken-1">
      {{ mode }} Perusahaan
      <v-spacer></v-spacer>
      <v-btn small icon @click="close(null)">
        <v-icon>mdi-close</v-icon>
      </v-btn> </v-card-title
    >
    <v-data-table
      v-if="table.items.length > 0"
      :loading="table.loading"
      :headers="table.headers"
      :items="table.items"
      disable-sort
    >
    </v-data-table>
    <p v-else class="text-center pb-6 title">Tidak ada anggota</p>

  </v-card>
</template>
<script>
import axios from "axios";
export default {
  name: "formUserCompany",
  props: ["data"],
  computed: {
    reload() {
      this.fetch();
    },
  },
  data: () => ({
    table: {
      headers: [
        {
          text: "Nama",
          value: "name",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "Email",
          value: "email",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "No HP",
          value: "phone",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "Job",
          value: "job",
          divider: true,
          class: "red-background white--text",
        },
      ],
      items: [],
      loading: false,
    },
    message: "",
    mode: "",
    loadingBtn: false,
    textSnackbar: "",
    snackbar: false,
  }),
  methods: {
    close(message) {
      this.$emit("close", message);
    },
    fetch() {
      this.mode = "Anggota";
      this.table.items = this.data;
      if(this.data == null){
        this.table.items = [];
      }
    },
  },
};
</script>
