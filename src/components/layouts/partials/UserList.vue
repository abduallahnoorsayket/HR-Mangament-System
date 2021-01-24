<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
              </div>
              <div>
                User List
                <router-link :to="{ path: '/create-new-user' }">
                  <a class="float" style="position: absolute; right: 0">
                    <button type="button" class="btn btn-success" style="font-size: 15px">
                      <span class="fas fa-plus-square"></span> Add New User
                    </button>
                  </a>
                </router-link>
              </div>
            </div>
          </div>
        </div>
        <div id="accordion" class="acordinContainer">
          <div class="card">
            <div id="headingOne" class="">
              <div class="row">
                <div class="col-md-12">
                  <div class="headerContaier">
                    <div class="float-left">
                      <h5>Advanced Search</h5>
                    </div>

                    <div class="float-right">
                      <i id="addIcon" class="fa fa-chevron-circle-down" style="font: 45px" data-toggle="collapse" data-target="#collapseOne1" aria-expanded="true" aria-controls="collapseOne"></i>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div data-parent="#accordion" id="collapseOne1" aria-labelledby="headingOne" class="collapse">
              <div class="card-body">
                <form action="" @submit.prevent="searchUser">
                  <div class="form-row">
                    <div class="col-md-4">
                      <label>Name</label>
                      <div class="form-group">
                        <input class="form-control" type="text" name="" id="" v-model="first_name" />
                      </div>
                    </div>

                    <div class="col-md-4">
                      <label>Email</label>
                      <div class="form-group">
                        <input class="form-control" type="email" name="" id="" v-model="username" />
                      </div>
                    </div>

                    <div class="col-md-4">
                      <label>Department</label>
                      <div class="form-group">
                        <select name="select" id="department" v-model="department" class="form-control">
                          <option value="0" selected>select department</option>
                          <option v-for="dprtmnt in all_departments" :key="dprtmnt.id" :value="dprtmnt.id">
                            {{ dprtmnt.department_name }}
                          </option>
                        </select>
                      </div>
                    </div>
                  </div>
                  <div class="form-row">
                    <div class="col-md-4">
                      <label>Group</label>
                      <div class="form-group">
                        <select name="select" id="department" v-model="groups" class="form-control">
                          <option value="" selected>Select Group</option>
                          <option v-for="grp in all_groups" :key="grp.id" :value="grp.id" selected>
                            {{ grp.name }}
                          </option>
                        </select>
                      </div>
                    </div>
                    <div class="col-md-4">
                      <label>Skills</label>
                      <div class="form-group">
                        <input class="form-control" type="text" name="" id="" v-model="skills" placeholder="Python, JavaScript, ..." />
                      </div>
                    </div>
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

        <!-- <div v-if="users"> -->
        <div class="card" v-for="user in users" :key="user.id">
          <div class="row">
            <div class="col-sm-2">
              <img v-if="user.user_profile && user.user_profile.image" :src="user.user_profile.image" class="rounded-circle" height="45" width="45" alt="user  image" />
              <img v-else width="45" height="45" class="rounded-circle" src="https://x1.xingassets.com/assets/frontend_minified/img/users/nobody_m.original.jpg" alt="" @error="$event.target.src = '../assets/images/avatars/default.png'" />

              <p class="mt-2">
                {{ user.first_name + " " + user.last_name }}
              </p>
            </div>

            <div class="vl"></div>

            <div class="col-sm-9">
              <div class="row mt-2">
                <div class="col-sm">
                  <label>Email:</label>
                  <br />{{ user.username }}
                </div>

                <div class="col-sm">
                  <label>Employee ID:</label>
                  <br />
                  <span v-if="user.user_profile">{{ user.user_profile.employee_id }}</span>
                </div>

                <div class="col-sm">
                  <label>Joining Date:</label>
                  <br />
                  <span v-if="user.user_profile">{{ user.user_profile.joining_date }}</span>
                </div>
                <div class="col-sm">
                  <label> Department</label>
                  <br />
                  <span v-if="user.user_profile">{{ user.user_profile.department.department_name }}</span>
                </div>
                <div class="col-sm">
                  <label> Branch</label>
                  <br />
                  <span v-if="user.user_profile && user.user_profile.branch">{{ user.user_profile.branch.branch_name }}</span>
                </div>

                <div class="col-sm mt-2">
                  <div class="float-right">
                    <div class="btn-group dropright">
                      <a data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" class="p-0 btn">
                        <i id="addIcon" class="fa fa-chevron-circle-down" style="font: 25px"></i>
                      </a>
                      <div tabindex="-1" role="menu" aria-hidden="true" class="dropdown-menu dropdown-menu-right">
                        <router-link class="dropdown-item" :to="{
                            name: 'UserDetails',
                            params: { id: user.id },
                          }">View
                        </router-link>

                        <router-link class="dropdown-item" :to="{
                            name: 'UpdateUser',
                            params: { id: user.id },
                          }">Edit
                        </router-link>
                        <a class="dropdown-item" href="#" @click="deleteData(user.id) in users">Delete</a>
                      </div>
                    </div>
                  </div>
                </div>
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
import axios from "axios";
import Master from "../Master";
import Swal from "sweetalert2";
import "../authheader";
// import Pagination from "./Pagination";

export default {
  name: "UserList",
  components: { Master },
  data() {
    return {
      first_name: null,
      last_name: null,
      full_name: null,
      username: null,
      department: null,
      branch: null,
      groups: null,
      skills: null,

      all_departments: null,
      all_branchs: null,
      all_groups: null,
      users: null,

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
    console.log("created");
  },
  updated() {
    this.initialLoad();
    console.log("updated");
  },

  methods: {
    initialLoad: function () {
      this.loadUsers();
      this.loadDepartment();
      this.loadGroup();
      this.resetOptions();
      this.loadBranch();
    },
    resetOptions: function () {},

    loadUsers: function () {
      var queryParam = {
        first_name: this.$route.query.first_name,
        username: this.$route.query.username,
        page: this.$route.query.page,
        department: this.$route.query.department,
        groups: this.$route.query.groups,
        skills: this.$route.query.skills
          ? "{" + this.$route.query.skills + "}"
          : null,
      };
      axios
        .get("users/", {
          params: queryParam,
        })
        .then((response) => {
          this.users = response.data.results;
          this.pagination.count = response.data.count;
          this.pagination.next = response.data.next;
          this.pagination.previous = response.data.previous;
          this.pagination.showing = response.data.results.length;
        });
    },
    loadDepartment() {
      const token = localStorage.getItem("token");
      axios
        .get("departments/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_departments = response.data;
          this.all_departments = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    loadGroup() {
      const token = localStorage.getItem("token");
      axios
        .get("groups/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_groups = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    loadBranch() {
      const token = localStorage.getItem("token");
      axios
        .get("branches/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_branchs = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    deleteData: function (id) {
      const currentUser = localStorage.getItem("id");
      console.log(currentUser);
      if (id == currentUser) {
        Swal.fire({
          icon: "error",
          title: "Sorry...",
          text: "Something went wrong! You can not delete yourself!",
        });
      } else {
        Swal.fire({
          title: "Are you sure?",
          text: "You won't be able to revert this!",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, delete it!",
        }).then((result) => {
          if (result.isConfirmed) {
            axios.delete("users/" + id + "/").then((response) => {
              if (response.status == 204) {
                this.loadUsers();
              }
            });
            Swal.fire("Deleted!", "Your user has been deleted.", "success");
          } else {
            Swal.fire("Cancelled", "Your user has not been deleted !", "error");
          }
        });
      }
    },
    setPage(page) {
      this.pagination.page = page;
      this.searchUser();
    },
    searchUser() {
      console.log(this.searchName);
      console.log(this.searchMail);
      this.$router.push({
        path: "user-list",
        query: {
          first_name: this.first_name,
          username: this.username,
          department: this.department,
          groups: this.groups,
          skills: this.skills,
          page: this.pagination.page,
        },
      });

     
    },
  },
  // end of Methods
};
</script>
<style scoped>
/*@import url('https://fonts.googleapis.com/css?family=Montserrat|Roboto:300,400|Yellowtail');*/

@import url("https://fonts.googleapis.com/css?family=Varela+Round");

.vl {
  border: 1px solid #eeeeee;
}

.headerContaier {
  margin: 0px 20px 20px 20px;
}

.btn-base {
  background-color: #8be1db !important;
  color: white;
}

#addIcon {
  cursor: pointer;
}

.label {
  text-align: left;
}

/*table  design
    */

/* jjkljkljkjkjkjkjk */
.row {
  font-family: "Varela Round", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
  margin-bottom: 10px;
  /* margin-top: 10px; */
}

.card {
  margin-top: 30px;
  padding-top: 30px;
  margin-bottom: 30px;
}

.card-content p {
  padding-bottom: 3px;
}
</style>
