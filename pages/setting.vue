<template>
  <v-card class="mt-3">
    <v-row class="mb-md-0 mb-sm-2">
      <v-col class="ml-2 pr-md-1" md="3" sm="12">
        <v-text-field
          v-model="search"
          placeholder="Filter"
          label="Filter"
          outlined
          dense
        ></v-text-field>
      </v-col>
      <v-col class="pl-md-1" md="3" sm="5">
        <v-btn
          color="blue"
          class="mb-1 mt-1 white--text"
          @click="fetch"
          rounded
        >
          Search
          <v-icon right dark> mdi-magnify </v-icon>
        </v-btn>
        
      </v-col>
    </v-row>
    <v-data-table
      v-if="table.items.length > 0"
      :loading="table.loading"
      :headers="table.headers"
      :items="table.items"
      :search="search"
      disable-sort
    >
      <template v-slot:[`item.action`]="{ item }">
        <td class="text-right">
          <v-btn small icon color="grey" @click="showUserList(item.id)">
            <v-icon>mdi-list-box</v-icon>
          </v-btn>
          <v-btn small icon color="primary" @click="editItem(item.id)">
            <v-icon>mdi-pencil</v-icon>
          </v-btn>
          <v-btn small icon color="error" @click="deleteItem(item)">
            <v-icon>mdi-delete</v-icon>
          </v-btn>
        </td>
      </template>
    </v-data-table>
    <p v-else class="text-center pb-6 title">Data Kosong</p>
    <v-dialog
      v-model="modal.showModalForm"
      persistent
      :scrollable="false"
      width="400"
      ><form-setting
        @close="closeModal"
        :id="modal.id"
        :idUser="modal.idUser"
        :idCompany="modal.idCompany"
        :absenCount="modal.absenCount"
        :jam1="modal.jam1"
        :jam2="modal.jam2"
        :jam3="modal.jam3"
        :jam4="modal.jam4"
      ></form-setting
    ></v-dialog>

    <v-dialog
      v-model="showUserModal"
      persistent
      :scrollable="false"
      width="520"
      ><form-setting
        @close="closeUserModal"
        :data="settingModal"
      ></form-setting
    ></v-dialog>

  </v-card>
</template>

<script>
import axios from "axios";
import formSetting from "./form_setting/formSetting.vue"
import formUserCompany from "./form_company/formUserCompany.vue";
export default {
  components: { formSetting, formUserCompany },
  data: () => ({
    search: "",
    textSnackbar: "",
    snackbar: false,
    modal: {
      id: "",
      idUser: "",
      idCompany: "",
      absenCount: "",
      jam1: "",
      jam2: "",
      jam3: "",
      jam4: "",
      showModalForm: false,
    },
    settingModal: null,
    showUserModal: false,
    table: {
      headers: [
        {
          text: "Nama",
          value: "idUser.name",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "Perusahaan",
          value: "idCompany.name",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "No HP",
          value: "idUser.phone",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "Setting Absen /hari",
          value: "absenCount",
          divider: true,
          class: "red-background white--text",
        },
        {
          text: "Action",
          value: "action",
          align: "end",
          class: "red-background white--text",
        },
      ],
      items: [],
      loading: false,
    },
  }),
  mounted() {
    this.fetch();
  },
  methods: {
    async fetch() {
      const response = await axios.get("http://localhost:5000/settings");
      const data = response.data;

      this.table.items = data.data.reverse();
    },
    async editItem(id) {
      const { data } = await axios.get("http://localhost:5000/settings/" + id)
      this.modal.id = data.data.id;
      this.modal.idUser = data.data.idUser;
      this.modal.idCompany = data.data.idCompany;
      this.modal.absenCount = data.data.absenCount;
      this.modal.jam1 = data.data.jam1;
      this.modal.jam2 = data.data.jam2;
      this.modal.jam3 = data.data.jam3;
      this.modal.jam4 = data.data.jam4;
      this.modal.showModalForm = true;
    },
    deleteItem(item) {
      // handle delete logic
    },
    async showUserList(id){
      const { data } = await axios.get("http://localhost:5000/settings/" + id)
      this.settingModal = data.data;
      this.showUserModal = true;
    },
    closeModal(message) {
      if (message != null) {
        this.textSnackbar = message;
        this.snackbar = true;
      }
      this.modal.showModalForm = false;
      // this.modal.showModalDelete = false;
      this.modal.id = "";
      this.modal.name = "";
      this.modal.address = "";
      this.fetch();
    },
    closeUserModal(){
      this.showUserModal = false;
    },
  },
};
</script>

<style>
.red-background {
  background-color: #b90000cc !important;
}
</style>