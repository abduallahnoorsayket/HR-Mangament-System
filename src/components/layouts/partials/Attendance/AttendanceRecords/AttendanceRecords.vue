<template >
  <Master>
    <template v-slot:content>
      <div class="app-main__inner" id="recordsModal">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Working Records</div>
            </div>
          </div>
        </div>
        <div></div>

        <div id="accordion" class="acordinContainer">
          <div class="card">
            <div id="headingOne" class="">
              <div class="row">
                <div class="col-md-12">
                  <div class="headerContaier">
                    <div class="float-left">
                      <h6>Adavanced Search</h6>
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
                    <div class="col-md-3" v-if="hasPermission('view_department')">
                      <label>Department *</label>
                      <div class="form-group">
                        <select name="select" id="department" v-model="department" @change="getEmployee()" class="form-control" required>
                          <option value="" selected>select department</option>
                          <option v-for="dprtmnt in all_departments" :key="dprtmnt.id" :value="dprtmnt.id">
                            {{ dprtmnt.department_name }}
                          </option>
                        </select>
                      </div>
                    </div>

                    <div class="col-md-3" v-if="hasPermission('view_user')">
                      <label>Employee *</label>
                      <div class="form-group">
                        <select name="select" id="emp" v-model="employee" class="form-control" required>
                          <option value="" selected>select employee</option>
                          <option v-for="employee in all_employees" :key="employee.id" :value="employee.id" selected>
                            {{ employee.first_name }}
                          </option>
                        </select>
                      </div>
                    </div>

                    <div class="col-md-3">
                      <label>Year *</label>
                      <div class="form-group">
                        <select name="select" id="year" v-model="AttendanceYear" class="form-control" required>
                          <option value="" selected>select year</option>
                          <option v-for="year in all_years" :key="year.index" :value="year.attendance_datetime__year" selected>
                            {{ year.attendance_datetime__year }}
                          </option>
                        </select>
                      </div>
                    </div>
                    <div class="col-md-3">
                      <label>Month</label>
                      <div class="form-group">
                        <select name="select" id="month" v-model="AttendanceMonth" class="form-control">
                          <option value="" selected>select month</option>
                          <option v-for="item in attendance_months" :key="item.id" :value="item.numeric">
                            {{ item.name }}
                          </option>
                        </select>
                      </div>
                    </div>
                  </div>
                  <div class="form-row">
                    <div class="col-md-12">
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
        <div class="row" v-if="newAttdanceList">
          <div class="col-12">
            <div class="main-card card">
              <div class="text-left ml-4">
                <h4>Attendance Records</h4>
                <br />
                <!-- <p v-for="(attendance, index) in newAttdanceList" :key="index">
                  <span v-if="index <= 1">
                    <span v-if="attendance.employee <= attendance.employee">
                      Employee name: {{ attendance.employee.first_name }}
                      <br />
                    </span>
                  </span>
                </p> -->
              </div>
              <div id="navbar-example">
                <!-- Nav tabs -->
                <ul class="nav nav-tabs">
                  <li class="nav-item" v-for="(item, index) in attendance_months" :key="index">
                    <a class="nav-link" data-toggle="tab" :class="[activeMonth == item.numeric ? 'active' : nulll,AttendanceMonth && activeMonth != item.numeric
                          ? 'disabled'
                          : null,
                      ]" role="tab" @click="
                        generateAttendanceList(item.numeric, AttendanceYear)
                      " :href="'#' + item.name">
                      {{ item.name }}</a>
                  </li>
                </ul>
                <!-- Tab panes {Fade}  -->
                <AddAttendanceModal @close-modal="CloseModal" v-if="isModalVisible" :employee_id="{
                    employee: employee,
                    isModalVisible: isModalVisible,
                  }" />
                <UpdateAttendanceModal @close-update="CloseUpdateModal" v-if="isUpdateVisible" :employee_update="{
                    update_attendance_id: update_attendance_id,
                    isUpdateVisible: isUpdateVisible,
                  }" />
                <div>
                  <div class="tab-content">
                    <div class="">
                      <div class="card-body">
                        <div class="table-responsive">
                          <table class="table table-striped">
                            <thead>
                              <tr>
                                <th scope="col">SL</th>
                                <th scope="col">Date</th>
                                <th scope="col">Day</th>
                                <th scope="col">Login</th>
                                <th scope="col">Logout</th>
                                <th scope="col">Total Breaktime</th>
                                <th scope="col">Total Working</th>
                                <th scope="col">Remarks</th>
                                <th scope="col">Cost</th>
                                <th scope="col" width="90px" v-if="hasPermission('add_attendancerecord')">
                                  Action
                                </th>
                              </tr>
                            </thead>
                            <tbody>
                              <tr v-for="(Attdance, index) in newAttdanceList" :key="index">
                                <td>{{ index + 1 }}</td>
                                <td>{{ Attdance.date }}</td>
                                <td>{{ Attdance.day }}</td>
                                <td>{{ Attdance.login_time }}</td>
                                <td>{{ Attdance.logout_time }}</td>
                                <td>{{ Attdance.total_breaktime }}</td>
                                <td>{{ Attdance.total_working }}</td>
                                <td>{{ Attdance.remarks }}</td>
                                <td>{{ Attdance.cost }}</td>

                                <template v-if="hasPermission('add_attendancerecord')">
                                  <td class="td_action" v-if="
                                      Attdance.login_time ||
                                      Attdance.logout_time ||
                                      Attdance.total_working ||
                                      Attdance.total_breaktime ||
                                      Attdance.remarks ||
                                      Attdance.cost
                                    ">
                                    <i class="fas fa-edit" style="color: 3f6ad8; padding: 8px" @click="OpenUpdateModal(Attdance.id)"></i>
                                  </td>
                                  <td class="td_action" v-else>
                                    <i class="fas fa-plus-square" style="color: #3ac47d; padding: 8px" @click="OpenModal()"></i>
                                  </td>
                                </template>
                              </tr>
                            </tbody>
                          </table>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <!-- table  -->
            </div>
          </div>
        </div>
        <div class="row" v-else>
          <div class="col-12">
            <div class="main-card card">
              <div class="text-left ml-4">
                <h4>Attendance Records</h4>
                <br />
              </div>
            </div>
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import Master from "../../../Master";
import Swal from "sweetalert2";
import axios from "axios";
import permissions from "../../../../../authorization/permissions";
import AddAttendanceModal from "./AddAttendanceModal";
import UpdateAttendanceModal from "./UpdateAttendanceModal";

export default {
  name: "AttendanceRecords",

  components: {
    Master,
    AddAttendanceModal,
    UpdateAttendanceModal,
  },
  data() {
    return {
      update_attendance_id: null,
      isModalVisible: false,
      isUpdateVisible: false,
      department: null,
      employee: null,
      all_departments: null,
      all_employees: null,
      all_branchs: null,
      all_groups: null,
      users: null,
      AttdanceList: [],
      newAttdanceList: [],
      AttendanceYear: null,
      AttendanceMonth: null,
      all_years: null,
      activeMonth: null,
      attendance_months: [
        { name: "January", numeric: 1 },
        { name: "February", numeric: 2 },
        { name: "March", numeric: 3 },
        { name: "April", numeric: 4 },
        { name: "May", numeric: 5 },
        { name: "June", numeric: 6 },
        { name: "July", numeric: 7 },
        { name: "August", numeric: 8 },
        { name: "September", numeric: 9 },
        { name: "October", numeric: 10 },
        { name: "November", numeric: 11 },
        { name: "December", numeric: 12 },
      ],
      pagination: {
        count: null,
        next: null,
        previous: null,
        showing: 0,
        page: null,
      },

      organised_data: {},
    };
  },

  created() {
    this.initialLoad();
    this.newAttdanceList = null;
    // this.update_attendance_id = null;
    console.log("created");
  },
  updated() {
    this.initialLoad();
    this.newAttdanceList = null;
    this.organised_data = {};
    console.log("updated");
  },

  methods: {
    OpenModal() {
      this.isModalVisible = !this.isModalVisible;
    },
    CloseModal(isModalVisible) {
      this.isModalVisible = isModalVisible;
    },
    OpenUpdateModal(id) {
      this.isUpdateVisible = !this.isUpdateVisible;
      this.update_attendance_id = id;
    },
    CloseUpdateModal(isUpdateVisible) {
      this.isUpdateVisible = isUpdateVisible;
    },
    initialLoad: function () {
      this.loadDepartment();
      this.loadYear();
    },
    getAttendanceList: function () {
      var queryParam = {
        department: this.$route.query.department,
        employee: this.$route.query.employee,
        year: this.$route.query.year,
        month: this.$route.query.month,
        page: this.$route.query.page,
      };
      axios
        .get("attendance-records/", {
          params: queryParam,
        })
        .then((response) => {
          this.AttdanceList = response.data.results;
          this.AttdanceList.map((x) => {
            this.organised_data[x.date] = x;
          });
          if (this.AttendanceMonth) {
            this.generateAttendanceList(
              this.AttendanceMonth,
              this.AttendanceYear
            );
            this.activeMonth = this.AttendanceMonth;
          } else {
            this.generateAttendanceList(
              this.attendance_months[0].numeric,
              this.AttendanceYear
            );
            this.activeMonth = this.attendance_months[0].numeric;
          }
        });
    },

    loadYear() {
      const token = localStorage.getItem("token");
      axios
        .get("years/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_years = response.data.data.years;
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    loadDepartment: function () {
      const token = localStorage.getItem("token");
      axios
        .get("departments/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then(
          function (response) {
            this.all_departments = response.data;
          }.bind(this)
        );
    },
    getEmployee: function () {
      axios
        .get("users/", {
          params: {
            department: this.department,
          },
        })
        .then(
          function (response) {
            this.all_employees = response.data.results;
          }.bind(this)
        );
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
    daysList(month, year) {
      this.newAttdanceList = [];

      let days = new Date(year, month, 0).getDate();

      let datelist = [];
      for (let i = 1; i <= days; i++) {
        datelist.push(i);
        let resultdate = new Date(`${year}-${month}-${i}`);
        let dd = resultdate.getDate();
        let mm = resultdate.getMonth() + 1;
        let yyyy = resultdate.getFullYear();
        if (dd < 10) {
          dd = "0" + dd;
        }
        if (mm < 10) {
          mm = "0" + mm;
        }
        resultdate = yyyy + "-" + mm + "-" + dd;
        if (
          Object.prototype.hasOwnProperty.call(this.organised_data, resultdate)
        ) {
          this.newAttdanceList.push(this.organised_data[resultdate]);
        } else {
          this.newAttdanceList.push({
            id: null,
            attendance_datetime: null,
            date: resultdate,
            day: null,
            month: null,
            year: null,
            login_time: null,
            logout_time: null,
            total_working: null,
            breakin_time: null,
            breakout_time: null,
            total_breaktime: null,
            remarks: null,
            cost: null,
          });
        }
      }

      return datelist;
    },
    generateAttendanceList(mths, yrs) {
      this.daysList(mths, yrs);
    },
    async searchUser() {
      await this.$router.push({
        path: "attendance-records",
        query: {
          department: this.department,
          employee: this.employee,
          year: this.AttendanceYear,
          month: this.AttendanceMonth,
          page: this.pagination.page,
        },
      });
      this.getAttendanceList();
    },

    // Permissions
    /*for Module(app)*/
    hasModulePermission(...module_name) {
      return permissions.hasModulePermission(...module_name);
      // return permissions.hasModulePermission.apply(...module_name)
    },

    /*for Model*/
    hasModelPermission(model_name) {
      return permissions.hasModelPermission(model_name);
    },

    /*for code_name*/
    hasPermission(permission_name) {
      return permissions.hasPermission(permission_name);
    },
  },
  // end of Methods
};
</script>
<style scoped>
@import url("https://fonts.googleapis.com/css?family=Varela+Round");

.vl {
  border: 1px solid #eeeeee;
}

.headerContaier {
  margin: 0px 20px 20px 20px;
}

.btn-base {
  color: #fff;
  background-color: #3ac47d;
  border-color: #3ac47d;
}

#addIcon {
  cursor: pointer;
}

.label {
  text-align: left;
}
/*table  design
            */
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
