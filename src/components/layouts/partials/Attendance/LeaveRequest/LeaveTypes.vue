<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-tape icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Leave Types</div>

              <router-link :to="{ path: '/create-leave-types' }">
                <a class="float" style="position: absolute; right: 0">
                  <button type="button" class="btn btn-success" style="font-size: 15px">
                    <span class="fas fa-plus-square"></span> Add Leave Type
                  </button>
                </a>
              </router-link>
            </div>
          </div>
        </div>
        <div id="accordion" class="acordinContainer">
          <div class="card">
            <div id="headingOne" class="">
              <div class="row">
                <div class="col-md-12">
                  <div class="headerContaier">
                    <div class="float-left"><h5>Advanced Search</h5></div>

                    <div class="float-right">
                      <i
                        id="addIcon"
                        class="fa fa-chevron-circle-down"
                        style="font: 45px"
                        data-toggle="collapse"
                        data-target="#collapseOne1"
                        aria-expanded="true"
                        aria-controls="collapseOne"
                      ></i>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div data-parent="#accordion" id="collapseOne1" aria-labelledby="headingOne" class="collapse">
              <div class="card-body">
                <form action="" @submit.prevent="searchLeaveType">
                  <div class="form-row">
                    <div class="col-md-4">
                      <label>Branch </label>
                      <div class="form-group">
                        <select name="select" id="branch" v-model="branch" class="form-control" @change="getLeaveType()">
                          <option value="" selected>Select Branch</option>
                          <option v-for="brnch in all_branches" :key="brnch.id" :value="brnch.id">
                            {{ brnch.branch_name }}
                          </option>
                        </select>
                      </div>
                    </div>

                    <div class="col-md-4">
                      <label for="exampleEmail11" class="">Leave Type </label>

                      <select name="select" id="type" v-model="leave_type" class="form-control">
                        <option value="" selected>Select Leave Type</option>
                        <option v-for="leavetype in all_leaveTypes" :key="leavetype.id" :value="leavetype.leave_type">
                          {{ leavetype.leave_type }}
                        </option>
                      </select>
                    </div>

                    <div class="col-md-4">
                      <label for="exampleEmail11" class=""> Leave Status</label>

                      <select name="select" id="type" v-model="leave_status" type="choice" class="form-control">
                        <!-- <option v-for="leavestatus in all_choices" :key="leavestatus.id" :value="leavestatus.id">
                                                  {{ leavestatus.LeaveType_Table }}
                                                </option> -->
                        <option value="" selected>Select Leave Status</option>
                        <option value="Paid">Paid</option>
                        <option value="Unpaid">Unpaid</option>
                      </select>
                    </div>
                  </div>

                  <div class="form-row">
                    <div class="col-md-8"></div>
                    <div class="col-md-4">
                      <label></label>
                      <br />
                      <br />

                      <div class="form-group">
                        <button type="submit" class="btn btn-base float-right">Search <i class="fa fa-search"></i>&nbsp;</button>
                      </div>
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div>

        <!--All Leave Types-->
        <div class="row">
          <div class="col-12">
            <div class="main-card card">
              <div class="card-body">
                <div class="mb-4">
                  <!-- <form action="" @submit.prevent="searchLeaveType">
                                      <div class="form-row">
                                        <div class="col-md-2"></div>
                                        <div class="col-md-3"></div>
                                        <div class="col-md-7">
                                          <div class="float-right">
                                            <button type="submit" class="btn btn-base float-right">Search <i class="fa fa-search"></i>&nbsp;</button>
                                          </div>
                                          <div class="float-right mr-2">
                                            <div class="form-group">
                                              <select name="select" id="branch" v-model="branch" class="form-control">
                                                <option value="false" disabled selected>Select Branch</option>
                                                <option v-for="brnch in all_branchs" :key="brnch.id" :value="brnch.id">
                                                  {{ brnch.branch_name }}
                                                </option>
                                              </select>
                                            </div>
                                          </div>
                                        </div>
                                      </div>
                                    </form> -->
                </div>
                <table class="table table-striped">
                  <thead>
                    <tr>
                      <th scope="col">SL</th>
                      <th scope="col">Types of Leave</th>
                      <th scope="col">Duration</th>
                      <th scope="col">Duration Type</th>
                      <th scope="col">Status</th>
                      <th scope="col">Payable Wages</th>
                      <th scope="col">Branch Name</th>
                      <th scope="col">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(leave_type, SL) in leave_types" :key="leave_type.id">
                      <td>{{ SL + 1 }}</td>
                      <td>{{ leave_type.leave_type }}</td>
                      <td>{{ leave_type.duration }}</td>
                      <td>{{ leave_type.duration_type }}</td>
                      <td>{{ leave_type.leave_status }}</td>
                      <td>{{ leave_type.payable_wages }}</td>

                      <td>
                        <span v-if="leave_type.branch.branch_name">
                          {{ leave_type.branch.branch_name }}
                        </span>
                      </td>

                      <td class="td_action">
                        <router-link class="item" :to="{ name: 'UpdateLeaveTypes', params: { id: leave_type.id } }">
                          <i class="fas fa-edit" style="color: #72e3da"></i>
                        </router-link>

                        <i
                          class="fas fa-trash-alt"
                          style="color: firebrick; padding: 10px"
                          @click="deleteLeaveTypeItem(leave_type.id) in leave_types"
                        >
                        </i>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
        <br />
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
            <!-- <form action="" @submit.prevent="searchUser">
                                              <input
                                                class="form-control"
                                                type="text"
                                                name=""
                                                id=""
                                                v-model="searchName"
                                              />
                                            </form> -->
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

export default {
  name: "LeaveTypes",
  components: { Master },
  data() {
    return {
      leave_types: null,
      searchByBranch: null,
      leave_type: null,
      leave_status: null,
      all_choices: null,
      all_branches: null,
      all_leaveTypes: null,
      branch: null,

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
    this.initialLoad();
  },
  updated() {
    this.initialLoad();
    console.log("updated");
  },
  methods: {
    initialLoad: function () {
      this.GetLeaveTypes();

      this.loadBranch();

      // this.loadChoices();
    },

    // loadChoices: function () {
    //   axios.get("choices/").then((response) => {
    //     this.all_choices = response.data;
    //     console.log(this.all_leaveTypes);
    //     console.log(response.status);
    //   });
    // },

    loadBranch() {
      const token = localStorage.getItem("token");
      axios
        .get("branches/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then(
          function (response) {
            this.all_branches = response.data;
          }.bind(this)
        );
    },
    getLeaveType: function () {
      axios
        .get("leave-types/", {
          params: {
            branch: this.branch,
          },
        })
        .then(
          function (response) {
            this.all_leaveTypes = response.data.results;
          }.bind(this)
        );
    },
    GetLeaveTypes: function () {
      var queryParam = {
        branch: this.$route.query.branch,
        leave_type: this.$route.query.leave_type,
        leave_status: this.$route.query.leave_status,
        page: this.$route.query.page,
      };
      axios
        .get("leave-types/", {
          params: queryParam,
        })
        .then((response) => {
          this.leave_types = response.data.results;
          this.pagination.count = response.data.count;
          this.pagination.next = response.data.next;
          this.pagination.previous = response.data.previous;
          this.pagination.showing = response.data.results.length;
          console.log(this.leave_types);
          console.log(response.status);
        });
    },

    deleteLeaveTypeItem: function (id) {
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
          axios.delete("leave-types/" + id + "/").then((response) => {
            if (response.status === 204) {
              this.GetLeaveTypes();
            }
          });
          Swal.fire("Deleted!", " leave type has been deleted!!", "success");
        } else {
          Swal.fire("Cancelled", " leave type not been deleted !", "error");
        }
      });
    },
    setPage(page) {
      this.pagination.page = page;
      this.searchLeaveType();
    },
    searchLeaveType() {
      console.log(this.branch);
      console.log(this.leave_status);
      this.$router.push({
        path: "leave-types",
        query: {
          branch: this.branch,
          leave_type: this.leave_type,
          leave_status: this.leave_status,

          page: this.pagination.page,
        },
      });
    },
  },
};
</script>

<style scoped>
.btn-base {
  color: #fff;
  background-color: #3ac47d;
  border-color: #3ac47d;
  margin-top: 2px;
}

.btn-base {
  color: #fff;
  background-color: #3ac47d;
  border-color: #3ac47d;
  margin-top: 2px;
}

.headerContaier {
  margin: 0px 20px 20px 20px;
}

.card {
  margin-top: 20px;
  padding-top: 20px;
  margin-bottom: 20px;
}
</style>
