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
        v-model="model.absenCount"
        label="Absen /hari"
        placeholder="Jumlah Absen /hari"
      >
        <template v-slot:append-outer>
          <v-btn small icon @click="editAbsenCount">
            <v-icon>mdi-pencil</v-icon>
          </v-btn>
        </template>
      </v-text-field>

      <!-- <v-text-field
        v-for="(textField, index) in textFields"
        :key="index"
        v-model="textFields[index]"
        label="Absen"
        placeholder="Masukkan absen"
        class="mt-3"
      ></v-text-field> -->

      <v-row class="mt-3">
        <v-col v-for="(textField, index) in textFields" :key="index" cols="6">
          <v-menu
            v-model="textField.menu"
            :close-on-content-click="false"
            :nudge-right="40"
            transition="scale-transition"
            offset-y
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="textFields[index].time"
                :label="'Jam ' + (index + 1)"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-time-picker
              v-model="textFields[index].time"
              :picker-width="300"
              :header-height="50"
            ></v-time-picker>
          </v-menu>
        </v-col>
      </v-row>

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
  name: "formSetting",
  props: [
    "id",
    "idUser",
    "idCompany",
    "absenCount",
    "jam1",
    "jam2",
    "jam3",
    "jam4",
  ],

  computed: {
    reload() {
      this.fetch();
    },
  },

  data: () => ({
    model: {
      idUser: "",
      idCompany: "",
      absenCount: "",
      jam1: "",
      jam2: "",
      jam3: "",
      jam4: "",
    },
    textFields: [],
    clockAbsenTmp: [],
    message: "",
    mode: "",
    loadingBtn: false,
    textSnackbar: "",
    snackbar: false,
  }),

  methods: {
    close(message) {
      this.model.idUser = "";
      this.model.idCompany = "";
      this.model.absenCount = "";
      this.model.jam1 = "";
      this.model.jam2 = "";
      this.model.jam3 = "";
      this.model.jam4 = "";
      this.$emit("close", message);
    },
    fetch() {
      this.mode = "Edit";
      this.model.idUser = this.idUser;
      this.model.idCompany = this.idCompany;
      this.model.absenCount = this.absenCount;
      this.model.jam1 = this.jam1;
      this.model.jam2 = this.jam2;
      this.model.jam3 = this.jam3;
      this.model.jam4 = this.jam4;
      this.clockAbsenTmp.push(this.jam1, this.jam2, this.jam3, this.jam4)
    },
    async proses() {
      this.loadingBtn = true;
      try {
        await axios.put("http://localhost:5000/settings/" + this.id, this.model);
        this.message = "Update Address Success";
        this.close(this.message);
      } catch (e) {
        this.textSnackbar = e.response.data.message;
        this.snackbar = true;
      }
      this.loadingBtn = false;
    },
    editAbsenCount() {
      this.textFields = []; // Reset array textFields

      const count = parseInt(this.model.absenCount, 10);

      if (count > 4) {
        // Jika count lebih dari 4
        this.showSnackbar('Tidak bisa lebih dari 4');
        return;
      }

      if (!isNaN(count)) {
        for (let i = 0; i < count; i++) {
          //isi textField dengan jam1 - jam4 yang ada di clockAbsenTmp (temporary)
          // this.textFields.push(this.clockAbsenTmp[i]);
          const time = this.clockAbsenTmp[i].substring(0,2) + ":" + this.clockAbsenTmp[i].substring(2);
          this.textFields.push({time: time, menu: false});
        }
      }
    },
    showSnackbar(message) {
      this.snackbar = true;
      this.textSnackbar = message;
    }
  },
};
</script>