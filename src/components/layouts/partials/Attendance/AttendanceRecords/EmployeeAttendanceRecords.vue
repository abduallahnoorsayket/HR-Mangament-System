<template>
  <!-- table  start  -->
  <div class="row" v-if="AttdanceList">
    <div class="col-12">
      <div class="main-card card">
        <div class="text-left ml-4">
          <h4 class="">Employee Attendance Records For Today</h4>
          <br />
        </div>
        <div id="navbar-example">
          <div class="card-body">
            <div class="table-responsive">
              <table class="mb-0 table table-bordered">
                <thead>
                  <tr>
                    <th scope="col">SL</th>
                    <th scope="col">Employee</th>
                    <th scope="col">Team</th>
                    <th scope="col">Date</th>
                    <th scope="col">Day</th>
                    <th scope="col">Login</th>
                    <th scope="col">Logout</th>
                    <th scope="col">Total Breaktime</th>
                    <th scope="col">Total Working</th>
                    <th scope="col">Remarks</th>
                    <th scope="col">Cost</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="(Attdance, index) in AttdanceList" :key="index">
                    <td>{{ index + 1 }}</td>
                    <td>
                      <span v-if="Attdance && Attdance.employee">
                        {{ Attdance.employee.first_name }}
                      </span>
                    </td>
                    <td v-if="Attdance && Attdance.employee.user_profile">
                      {{
                        Attdance.employee.user_profile.department
                          .department_name
                      }}
                    </td>
                    <td>{{ Attdance.date }}</td>
                    <td>{{ Attdance.day }}</td>
                    <td>{{ Attdance.login_time }}</td>
                    <td>{{ Attdance.logout_time }}</td>
                    <td>{{ Attdance.total_breaktime }}</td>
                    <td>{{ Attdance.total_working }}</td>
                    <td>{{ Attdance.remarks }}</td>
                    <td>{{ Attdance.cost }}</td>
                  </tr>
                </tbody>
              </table>
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
          <h4>Attendance Entry</h4>
          <br />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "AttendanceEntry",
  data() {
    return {
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
    console.log("created");
  },
  updated() {
    console.log("updated");
  },

  methods: {
    initialLoad: function () {
      this.getAttendanceList();
    },
    getAttendanceList: function () {
      axios.get("todays-employee-attendance-record/").then((response) => {
        this.AttdanceList = response.data;
        console.log("136=====================", this.AttdanceList);
        // this.AttdanceList.map((x) => {
        //   this.organised_data[x.date] = x;
        // });
        // this.generateAttendanceList();
      });
    },

    // daysList(month, year) {
    //   this.newAttdanceList = [];
    //   let days = new Date(year, month, 0).getDate();
    //   let datelist = [];
    //   for (let i = 1; i <= days; i++) {
    //     datelist.push(i);
    //     let resultdate = new Date(`${year}-${month}-${i}`);
    //     let dd = resultdate.getDate();
    //     let mm = resultdate.getMonth() + 1;
    //     let yyyy = resultdate.getFullYear();
    //     if (dd < 10) {
    //       dd = "0" + dd;
    //     }
    //     if (mm < 10) {
    //       mm = "0" + mm;
    //     }
    //     resultdate = yyyy + "-" + mm + "-" + dd;
    //     if (
    //       Object.prototype.hasOwnProperty.call(this.organised_data, resultdate)
    //     ) {
    //       this.newAttdanceList.push(this.organised_data[resultdate]);
    //     } else {
    //       this.newAttdanceList.push({
    //         id: null,
    //         attendance_datetime: null,
    //         date: resultdate,
    //         day: null,
    //         month: null,
    //         year: null,
    //         login_time: null,
    //         logout_time: null,
    //         total_working: null,
    //         breakin_time: null,
    //         breakout_time: null,
    //         total_breaktime: null,
    //         remarks: null,
    //         cost: null,
    //       });
    //     }
    //   }

    //   return datelist;
    // },
    // generateAttendanceList() {
    //   let Today = new Date();
    //   let mm = Today.getMonth() + 1;
    //   let yyyy = Today.getFullYear();
    //   this.daysList(mm, yyyy);
    // },
  },
  // end of Methods
};
</script>
<style scoped>
</style>
