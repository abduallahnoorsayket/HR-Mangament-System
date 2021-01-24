<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-user-friends icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Client List</div>
              <router-link :to="{ path: '/add-client' }">
                <a class="float" style="position: absolute; right: 0">
                  <button type="button" class="btn btn-success" style="font-size: 15px">
                    <span class="fas fa-plus-square"></span> Add Client
                  </button>
                </a>
              </router-link>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-12">
            <div class="main-card card">
              <div class="row">
                <div class="col-md-12 ">
                  <div class="searchContainer">
                    <div class="float-right">
                      <form action="" @submit.prevent="searchClient">
                        <button type="submit" class="btn  btn-info float-right "><i class="fa fa-search"></i>&nbsp;</button>
                        <div class="float-right">
                          <input class="form-control " type="text" placeholder="Seach by name" v-model="searchName" />
                        </div>
                      </form>
                    </div>
                  </div>
                </div>
              </div>
              <ViewClientModal @close-update="CloseUpdateModal" v-if="isUpdateVisible" :employee_update="{
                    update_attendance_id: update_attendance_id,
                    isUpdateVisible: isUpdateVisible,
                  }" />
              <div class="card-body">
                <table class="table table-striped">
                  <thead>
                    <tr>
                      <th scope="col">SL</th>
                      <th scope="col">Client Name</th>
                      <th scope="col">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(client, index) in all_clients" :key="index">
                      <td>{{ index + 1 }}</td>
                      <td>{{ client.client_name }}</td>
                      <td class="td_action">
                        <i class="fas fa-eye" style="color: #0b9aec; font-size: 20px; padding: 10px" @click="OpenUpdateModal(client.id)"></i>
                        <router-link class="item" :to="{name: 'UpdateClient', params: { id: client.id },}">
                          <i class="fas fa-edit" style="color: #72e3da;  font-size: 20px "></i>
                        </router-link>
                        <i class="fas fa-trash-alt " style="color: firebrick; font-size: 20px; padding: 10px" @click="deleteClient(client.id) in all_clients">
                        </i>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
        <div class="row">
          <div class="col-md-4">
            <ul class="pagination" v-if="pagination.count">
              <li class="page-item">
                <a href="#">Showing {{ pagination.showing }} of {{ pagination.count }}</a>
              </li>
              <li :class="{ disabled: !pagination.previous }" class="page-item">
                <a href="#!" v-on:click="setPage(pagination.previous)">Previous</a>
              </li>

              <li :class="{ disabled: !pagination.next }" class="page-item">
                <a href="#!" v-on:click="setPage(pagination.next)">Next</a>
              </li>
            </ul>
          </div>
          <div class="col-md-6"></div>
          <div class="col-md-2">
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import Master from "../../../Master";
import axios from "axios";
import Swal from "sweetalert2";
import "../../../authheader";
import ViewClientModal from "./ViewClientModal";

export default {
  name: "ClientList",
  components: { Master, ViewClientModal },
  data() {
    return {
      isUpdateVisible: false,
      update_attendance_id: null,
      searchName: null,
      all_clients: null,
      pagination: {
        count: null,
        next: null,
        previous: null,
        showing: 0,
        page: null,
      },
    };
  },
  created() {
    this.initialload();
    console.log("created");
  },
  updated() {
    this.initialload();
    console.log("updated");
  },
  methods: {
    OpenUpdateModal(id) {
      this.isUpdateVisible = !this.isUpdateVisible;
      this.update_attendance_id = id;
    },
    CloseUpdateModal(isUpdateVisible) {
      this.isUpdateVisible = isUpdateVisible;
    },
    initialload() {
      this.GetClientList();
    },
    setPage(page) {
      this.pagination.page = page;
      this.searchClient();
    },
    GetClientList: function () {
      var queryParam = {
        client_name: this.$route.query.client_name,
        page: this.$route.query.page,
      };
      axios
        .get("clients/", {
          params: queryParam,
        })
        .then((response) => {
          this.all_clients = response.data.results;
          this.pagination.count = response.data.count;
          this.pagination.next = response.data.next;
          this.pagination.previous = response.data.previous;
          this.pagination.showing = response.data.results.length;
        });
    },

    deleteClient: function (id) {
      Swal.fire({
        title: "Are you sure?",
        text: "You won't be able to revert this!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, delete it!",
      }).then((response) => {
        if (response.isConfirmed) {
          axios.delete("clients/" + id + "/").then((response) => {
            if (response.status === 204) {
              this.GetClientList();
            }
          });
          Swal.fire("Deleted!", "Client has been deleted!!", "success");
        } else {
          Swal.fire("Cancelled", "Client  has not been deleted !", "error");
        }
      });
    },
    searchClient() {
      this.$router.push({
        path: "client-list",
        query: {
          client_name: this.searchName,
          page: this.pagination.page,
        },
      });
    },
  },
};
</script>

<style scoped>
.table {
  text-align: center;
}
.btn-info {
  color: white;
  margin-left: 10px;
  margin-top: 2px;
}
.searchContainer {
  margin: 5px 25px;
}
.td_action button,
i {
  margin: 3px;
  cursor: pointer;
}
</style>