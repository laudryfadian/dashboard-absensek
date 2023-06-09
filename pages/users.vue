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
      width="700"
      ><form-users
        @close="closeModal"
        :id="modal.id"
        :name="modal.name"
        :email="modal.email"
        :password="modal.password"
        :phone="modal.phone"
        :salary="modal.salary"
        :is-absen="modal.isAbsen"
        :job="modal.job"
        :job-type="modal.jobType"
        :super-user="modal.superUser"
        :id-company="modal.idCompany"
      ></form-users
    ></v-dialog>

    <v-dialog
      v-model="showUserModal"
      persistent
      :scrollable="false"
      width="520"
      ><form-user-company
        @close="closeUserModal"
        :data="userModal"
      ></form-user-company
    ></v-dialog>

  </v-card>
</template>

<script>
import axios from "axios";
import formUsers from "./form_users/formUsers.vue"
import formUserCompany from "./form_company/formUserCompany.vue";
export default {
  components: { formUsers, formUserCompany },
  data: () => ({
    search: "",
    textSnackbar: "",
    snackbar: false,
    modal: {
      id: "",
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
      showModalForm: false,
    },
    userModal: null,
    showUserModal: false,
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
          text: "Perusahaan",
          value: "idCompany.name",
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
      const response = await axios.get("http://localhost:5000/users");
      const data = response.data;

      this.table.items = data.data.reverse();
    },
    async editItem(id) {
      const { data } = await axios.get("http://localhost:5000/users/" + id)
      this.modal.id = data.data.id;
      this.modal.name = data.data.name;
      this.modal.email = data.data.email;
      this.modal.password = data.data.password;
      this.modal.phone = data.data.phone;
      this.modal.job = data.data.job;
      this.modal.superUser = data.data.superUser;
      this.modal.salary = data.data.salary;
      this.modal.isAbsen = data.data.isAbsen;
      this.modal.jobType = data.data.jobType;
      this.modal.idCompany = data.data.idCompany;
      this.modal.showModalForm = true;
    },
    deleteItem(item) {
      // handle delete logic
    },
    async showUserList(id){
      const { data } = await axios.get("http://localhost:5000/users/company/" + id)
      this.userModal = data.data;
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