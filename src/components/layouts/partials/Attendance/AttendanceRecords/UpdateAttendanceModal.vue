<template>
  <div class="text-center mt-4"></div>
  <teleport to="body">
    <div v-if="isUpdateVisible" class="modal">
      <div class="main-card card">
        <div class="card-header">
          <p class="float-left">Edit Attendance Information</p>
        </div>
        <div class="card-body">
          <form @submit.prevent="submitEditAttendance" autocomplete="off" v-if="form_data">
            <div class="form-group">
              <div class="form-row">
                <div class="col-md-12">
                  <div class="position-relative form-group">
                    <label for="exampleEmail11" class="">Attendance Datetime
                    </label>
                    <input placeholder="attendance date time" id="Login" v-model="form_data.attendance_datetime" type="datetime-local" class="form-control" :class="{
                        'is-invalid':
                          AddAttendance_error &&
                          AddAttendance_error.attendance_datetime,}" />
                    <!--error handling-->
                    <div :class="{
                        'invalid-feedback' :
                          AddAttendance_error        &&
                          AddAttendance_error.attendance_datetime,
                      }" v-if="
                        AddAttendance_error &&
                        AddAttendance_error.attendance_datetime ">
                      {{ AddAttendance_error.attendance_datetime[0] }}
                    </div>
                  </div>
                </div>
              </div>
              <div class="form-row">
                <div class="col-md-6">
                  <div class="position-relative form-group">
                    <label for="exampleEmail11" class="">Login Time *</label>
                    <input placeholder="Login" id="Login" v-model="form_data.login_time" type="time" class="form-control" :class="{
                        'is-invalid':
                          AddAttendance_error && AddAttendance_error.login_time,
                      }" />

                    <!--error handling-->
                    <div :class="{
                        'invalid-feedback':
                          AddAttendance_error && AddAttendance_error.login_time,
                      }" v-if="
                        AddAttendance_error && AddAttendance_error.login_time
                      ">
                      {{ AddAttendance_error.login_time[0] }}
                    </div>
                  </div>
                </div>

                <div class="col-md-6 position-relative form-group">
                  <label>Breakin Time</label>
                  <input placeholder="Breakin" id="Breakin" v-model="form_data.breakin_time" type="time" class="form-control" :class="{
                      'is-invalid':
                        AddAttendance_error && AddAttendance_error.breakin_time,
                    }" />

                  <div :class="{
                      'invalid-feedback':
                        AddAttendance_error && AddAttendance_error.breakin_time,
                    }" v-if="
                      AddAttendance_error && AddAttendance_error.breakin_time
                    ">
                    {{ AddAttendance_error.breakin_time[0] }}
                  </div>
                </div>
              </div>

              <div class="form-row">
                <div class="col-md-6">
                  <div class="position-relative form-group">
                    <label for="exampleEmail11" class="">Breakout Time</label>
                    <input name="Start-date" id="Breakout" v-model="form_data.breakout_time" placeholder="" type="time" class="form-control" :class="{
                        'is-invalid':
                          AddAttendance_error &&
                          AddAttendance_error.breakout_time,
                      }" />
                    <div :class="{
                        'invalid-feedback':
                          AddAttendance_error &&
                          AddAttendance_error.breakout_time,
                      }" v-if="
                        AddAttendance_error && AddAttendance_error.breakout_time
                      ">
                      {{ AddAttendance_error.breakout_time[0] }}
                    </div>
                  </div>
                </div>

                <div class="col-md-6 position-relative form-group">
                  <label>Logout Time</label>
                  <input name="End-date" id="Logout" v-model="form_data.logout_time" placeholder="" type="time" class="form-control" :class="{
                      'is-invalid':
                        AddAttendance_error && AddAttendance_error.logout_time,
                    }" />
                  <div :class="{
                      'invalid-feedback':
                        AddAttendance_error && AddAttendance_error.end_date,
                    }" v-if="
                      AddAttendance_error && AddAttendance_error.logout_time
                    ">
                    {{ AddAttendance_error.logout_time[0] }}
                  </div>
                </div>
              </div>
              <div class="form-row">
                <div class="col-md-6">
                  <div class="position-relative form-group">
                    <label for="exampleEmail11" class=""> Remarks</label>
                    <input placeholder="Remarks" id="remarks" v-model="form_data.remarks" type="text" class="form-control" :class="{
                        'is-invalid':
                          AddAttendance_error && AddAttendance_error.remarks,
                      }" />

                    <!--error handling-->
                    <div :class="{
                        'invalid-feedback':
                          AddAttendance_error && AddAttendance_error.remarks,
                      }" v-if="AddAttendance_error && AddAttendance_error.remarks">
                      {{ AddAttendance_error.remarks[0] }}
                    </div>
                  </div>
                </div>
                <div class="col-md-6">
                  <div class="position-relative form-group">
                    <label for="exampleEmail11" class=""> Cost</label>
                    <input placeholder="Cost" id="cost" v-model="form_data.cost" type="number" class="form-control" :class="{
                        'is-invalid':
                          AddAttendance_error && AddAttendance_error.cost,
                      }" />

                    <!--error handling-->
                    <div :class="{
                        'invalid-feedback':
                          AddAttendance_error && AddAttendance_error.cost,
                      }" v-if="AddAttendance_error && AddAttendance_error.cost">
                      {{ AddAttendance_error.cost[0] }}
                    </div>
                  </div>
                </div>
              </div>
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-6"></div>
                  <div class="col-md-6">
                    <button class="mt-2 float-right ml-2 btn btn-secondary" @click="CloseUpdateModal()">
                      Close
                    </button>
                    <button @submit.prevent="submitEditAttendance" class="mt-2 btn btn-success float-right">
                      Submit
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
// import permissions from "../../../../../authorization/permissions";
export default {
  name: "UpdateAttendanceModal",
  props: ["employee_update"],
  data() {
    return {
      update_attendance_id: null,
      modalOpen: false,
      isModalVisible: false,
      isUpdateVisible: false,
      form_data: null,
      attendance_datetime: null,
      employee: null,
      login_time: null,
      breakin_time: null,
      breakout_time: null,
      logout_time: null,
      remarks: null,
      cost: null,
      user: null,
      users: null,
      AddAttendance_error: {
        attendance_datetime: null,
        employee: null,
        login_time: null,
        breakin_time: null,
        breakout_time: null,
        logout_time: null,
        remarks: null,
        cost: null,
      },
    };
  },
  created() {
    this.update_attendance_id = this.$props.employee_update.update_attendance_id;
    this.isUpdateVisible = this.$props.employee_update.isUpdateVisible;
    this.getAttendanceData();
  },

  updated() {
    // this.form_data = null;
  },
  methods: {
    CloseUpdateModal() {
      this.isUpdateVisible = !this.isUpdateVisible;
      this.$emit("close-update", this.isUpdateVisible);
    },
    getAttendanceData: function () {
      axios.get("attendance-records/" + this.update_attendance_id + "/").then(
        (response) => {
          this.form_data = response.data;
          this.form_data.employee = this.form_data.employee.id;
        },
        (response) => {
          console.log("----", response);
        }
      );
    },
    submitEditAttendance() {
      axios
        .put("attendance-records/" + this.update_attendance_id + "/", {
          attendance_datetime: this.form_data.attendance_datetime,
          employee: this.form_data.employee,
          login_time: this.form_data.login_time,
          breakin_time: this.form_data.breakin_time,
          breakout_time: this.form_data.breakout_time,
          logout_time: this.form_data.logout_time,
          remarks: this.form_data.remarks,
          cost: this.form_data.cost,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text: "You have successfully Edit Attendance ..",
          }).then((response) => {
            this.$router.push({ name: "AttendanceRecords" });
            this.$router.go();
            console.log(response);
          });

          console.log(response);
        })
        .catch((error) => {
          this.AddAttendance_error = error.response.data;
          console.log("--++", error.response);
        });
    },
  },
};
</script>
<style>
/* teleport modal */
.modal {
  /* position: absolute; */
  top: 20;
  right: 0;
  bottom: 20;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.btn-close {
  float: right !important;
  border: none;
  font-size: 20px;
  padding: 20px;
  cursor: pointer;
  font-weight: bold;
  color: #4aae9b;
  background: transparent;
}
</style>