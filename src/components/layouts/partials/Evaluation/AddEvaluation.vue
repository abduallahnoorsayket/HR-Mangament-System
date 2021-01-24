<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="pe-7s-calculator icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Add Evaluation</div>
            </div>
          </div>
        </div>
        <div class="main-card card" v-if="form_data">
          <div class="card-body">
            <div v-if="error_data && error_data.non_field_errors" class="alert alert-danger alert-dismissible fade show text-center " role="alert">
              The Evaluation of this Employee for this month of this year is alredy added. Plese select a different month.
              <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                <span aria-hidden="true">&times;</span>
              </button>
            </div>
            <p>
              <strong class="app-sidebar__heading">Employee Information</strong>
            </p>
            <hr />
            <form @submit.prevent="submitEvaluationForm">
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-4">
                    <div class="position-relative form-group">
                      <label>Department <span class="asterisk">*</span></label>
                      <div class="form-group">
                        <select name="select" id="department" v-model="department" @change="getEmployee()" class="form-control" required>
                          <option value="" selected>select department</option>
                          <option v-for="dprtmnt in all_departments" :key="dprtmnt.id" :value="dprtmnt.id">
                            {{ dprtmnt.department_name }}
                          </option>
                        </select>
                      </div>
                    </div>
                  </div>
                  <div class="col-md-4">
                    <div class="position-relative form-group">
                      <label>Employee <span class="asterisk">*</span></label>
                      <div class="form-group">
                        <select name="select" id="emp" v-model="employee" class="form-control" :class="{ 'is-invalid': error_data && error_data.employee}">
                          <option value="" selected>select employee</option>
                          <option v-for="employee in all_employees" :key="employee.id" :value="employee.id" selected>
                            {{ employee.first_name }}
                          </option>
                        </select>
                        <div :class="{ 'invalid-feedback': error_data && error_data.employee }" v-if="error_data && error_data.employee">
                          {{ error_data.employee[0] }}
                        </div>
                      </div>
                    </div>
                  </div>

                  <div class="col-md-4">
                    <div class="position-relative form-group">
                      <label>Current Date </label>
                      <input name="Start-date" id="Current" placeholder="" type="text" v-model="today" class="form-control" disabled />
                    </div>
                  </div>
                  <div class="col-md-4">
                    <div class="position-relative form-group">
                      <label>Month of Evalutation
                        <span class="asterisk">*</span></label>
                      <div class="form-group">
                        <select name="select" id="month" v-model="month_of_evaluation" class="form-control" :class="{ 'is-invalid': error_data && error_data.month_of_evaluation}">
                          <option value="" selected>select month</option>
                          <option v-for="item in attendance_months" :key="item.id" :value="item.numeric">
                            {{ item.name }}
                          </option>
                        </select>
                        <div :class="{ 'invalid-feedback': error_data && error_data.month_of_evaluation }" v-if="error_data && error_data.month_of_evaluation">
                          {{ error_data.month_of_evaluation[0] }}
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="col-md-4">
                    <div class="position-relative form-group">
                      <label>
                        Year of Evalutation
                        <span class="asterisk">*</span></label>
                      <div class="form-group">
                        <select name="select" id="year" v-model="year_of_evaluation" class="form-control" :class="{ 'is-invalid': error_data && error_data.year_of_evaluation}">
                          <option value="" selected>select year</option>
                          <option v-for="year in all_years" :key="year.index" :value="year.attendance_datetime__year" selected>
                            {{ year.attendance_datetime__year }}
                          </option>
                        </select>
                        <div :class="{ 'invalid-feedback': error_data && error_data.year_of_evaluation }" v-if="error_data && error_data.year_of_evaluation">
                          {{ error_data.year_of_evaluation[0] }}
                        </div>
                      </div>
                    </div>
                  </div>

                  <div class="col-md-4">
                    <div class="position-relative form-group">
                      <label>Filled By</label>
                      <input placeholder="" id="filled" type="text" v-model="form_data.first_name" class="form-control" disabled />
                    </div>
                  </div>
                  <div class="col-md-12">
                    <div class="position-relative form-group mt-4">
                      <div>
                        <label><strong class="app-sidebar__heading">Employee Ratings</strong></label>
                        <hr />
                      </div>
                      <div class="table-responsive">
                        <table class="table table-striped">
                          <thead>
                            <tr>
                              <th scope="col" style="width:25%"></th>
                              <th scope="col" v-for="(point, index) in all_rating_points" :key="index">
                                {{ point.value}}-{{ point.name }}
                              </th>
                            </tr>
                          </thead>
                          <tbody>
                            <template v-for="(rating, index) in all_ratings" :key="index" >
                              <tr v-if="rating.status === true">
                                <td>
                                  {{ rating.name }}<span class="asterisk"> *</span>
                                </td>
                                <td v-for="(point, index) in all_rating_points" :key="index">
                                  <div class="form-check">
                                    <input class="form-check-input" type="radio" :name="rating.name" :id="rating.id+'_'+point.id" :value="1" @change="ratingPoint(rating.id,point.id)" required />
                                  </div>
                                </td>
                              </tr>
                            </template>
                          </tbody>
                        </table>
                      </div>
                    </div>
                  </div>
                  <div class="col-md-12">
                    <div class="position-relative form-group mt-4">
                      <div>
                        <label><strong class="app-sidebar__heading">Additional Comments</strong></label>
                        <hr />
                      </div>
                      <textarea name="Additional Comments" id="Comments" placeholder="Additional Comments" type="text" class="form-control" v-model="additional_comments" rows="4" cols="50"></textarea>
                    </div>
                  </div>
                </div>
                <div class="form-group">
                  <div class="form-group">
                    <button @submit.prevent="submitEvaluationForm" class="btn btn-success btn-lg float-right">
                      Submit
                    </button>
                  </div>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import Master from "../../Master";
import axios from "axios";
import Swal from "sweetalert2";

export default {
  name: "AddEvaluation",
  components: { Master },

  data() {
    return {
      month_of_evaluation: null,
      year_of_evaluation: null,
      ratingData: {},
      finalratingData: [],
      form_data: {
        first_name: null,
      },
      user: null,
      today: null,
      department: null,
      employee: null,
      all_departments: null,
      all_employees: null,
      all_years: null,
      all_rating_points: null,
      all_ratings: null,
      additional_comments: null,
      error_data: {
        employee: null,
        month_of_evaluation: null,
        year_of_evaluation: null,
        additional_comments: null,
        employee_information_employee_rating: null,
      },
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
    };
  },
  created() {
    this.initialLoad();
    console.log("created");
  },
  updated() {
    this.initialLoad();
  },

  methods: {
    ratingPoint(ratingid, pointid) {
      this.ratingData[ratingid] = pointid;
    },
    initialLoad: function () {
      this.loadDepartment();
      this.loadYear();
      this.loadRatingPoints();
      this.loadRatings();
      this.getToday();
      this.loadUser();
    },
    loadRatingPoints: function () {
      const token = localStorage.getItem("token");
      axios
        .get("rating-points/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_rating_points = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    loadRatings: function () {
      const token = localStorage.getItem("token");
      axios
        .get("ratings/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_ratings = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    loadUser: function () {
      let user_id = localStorage.getItem("id");
      const token = localStorage.getItem("token");
      axios
        .get("users/" + user_id + "/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.form_data = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    getToday() {
      let resultdate = new Date();
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
      this.today = resultdate;
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
    PrcessRatingdata() {
      this.finalratingData = [];
      for (const [key, value] of Object.entries(this.ratingData)) {
        this.finalratingData.push({ rating: key, rating_point: value });
      }
    },
    async submitEvaluationForm() {
      const token = localStorage.getItem("token");
      await this.PrcessRatingdata();
      axios
        .post("employee-evaluation/", {
          headers: {
            Authorization: `token ${token}`,
          },
          employee: this.employee,
          month_of_evaluation: this.month_of_evaluation,
          year_of_evaluation: this.year_of_evaluation,
          additional_comments: this.additional_comments,
          employee_information_employee_rating: this.finalratingData,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text: "You have successfully Added  Evalution.",
          }).then((result) => {
            this.$router.push("add-evaluation");
            this.$router.go();
            console.log(result);
          });
          console.log(response);
        })
        .catch((error) => {
          this.error_data = error.response.data;
          console.log("--++", error.response);
        });
    },
    // end of submit
  },
  // end of method
};
</script>

<style scoped>
.asterisk {
  color: red;
}
hr {
  height: 1px;
  background-color: #dcdcdc;
  border: none;
  margin-bottom: 20px;
  margin-top: 0px;
}
.table th .table td {
  vertical-align: top !important;
  border-collapse: collapse !important;
}
.form-check-input {
  position: none;
  margin-top: -0.3rem;
  margin-left: -0.9rem;
}
</style>