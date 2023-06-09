<template>
  <v-container>
    <!-- Row 1 -->
    <v-row>
      <!-- Column 1: Title -->
      <v-col cols="3">
        <h3>Versi Aplikasi</h3>
      </v-col>

      <!-- Column 2: Textfield -->
      <v-col cols="3">
        <v-text-field v-model="items.versi1" label="Row 1 - Column 2"></v-text-field>
      </v-col>

      <!-- Column 3: Textfield -->
      <v-col cols="3">
        <v-text-field v-model="items.versi2" label="Row 1 - Column 3"></v-text-field>
      </v-col>

      <!-- Column 4: Textfield -->
      <v-col cols="3">
        <v-text-field v-model="items.versi3" label="Row 1 - Column 4"></v-text-field>
      </v-col>
    </v-row>

    <!-- Row 2 -->
    <v-row>
      <!-- Column 1: Title -->
      <v-col cols="3">
        <h3>Face Recog Model</h3>
      </v-col>

      <!-- Column 2: Textfield -->
      <v-col cols="3">
        <v-text-field v-model="items.faceRecogModel" label="Row 2 - Column 2"></v-text-field>
      </v-col>
    </v-row>

    <!-- Row 3 -->
    <v-row>
      <!-- Column 1: Title -->
      <v-col cols="3">
        <h3>Maintenance</h3>
      </v-col>

      <!-- Column 2: Button -->
      <v-col cols="9">
        <v-switch
          v-model="items.maintenance"
          hide-details
          inset
        ></v-switch>
      </v-col>
    </v-row>

    <!-- Row 4 -->
    <v-row>
      <!-- Save Button -->
      <v-col>
        <v-btn color="primary">Save</v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      items: {
        id: "6482bd7de1d4d627f400e92f",
        versi1: "",
        versi2: "",
        versi3: "",
        faceRecogModel: "",
        maintenance: false,
      }
    };
  },
  mounted() {
    this.fetch();
  },
  methods: {
    async fetch() {
      const response = await axios.get("http://localhost:5000/config/" + this.items.id)
      const data = response.data.data;

      this.items.versi1 = data.versi1;
      this.items.versi2 = data.versi2;
      this.items.versi3 = data.versi3;
      this.items.faceRecogModel = data.faceRecogModel;
      this.items.maintenance = data.maintenance;
    },
    clear() {
      this.items.versi1 = "";
      this.items.versi2 = "";
      this.items.versi3 = "";
      this.items.faceRecogModel = "";
      this.items.maintenance = false;
    }
  }
};
</script>
