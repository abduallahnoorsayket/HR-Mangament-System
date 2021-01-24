<template>
  <div class="mt-2"></div>
  <teleport to="body">
    <div v-if="isModalVisible" class="modal">
      <div>
        <div class="main-card card">
          <div class="card-header">
            <p class="float-left">Add Attendance Information</p>
          </div>
          <div class="card-body">
            <form @submit.prevent="submitAddAttendance" autocomplete="off">
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-12">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Attendance Datetime
                      </label>
                      <input placeholder="attendance date time" id="Login" v-model="attendance_datetime" type="datetime-local" class="form-control" :class="{
                          'is-invalid':
                            AddAttendance_error &&
                            AddAttendance_error.attendance_datetime,
                        }" />

                      <!--error handling-->
                      <div :class="{
                          'invalid-feedback':AddAttendance_error && AddAttendance_error.attendance_datetime, }" v-if="
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
                      <input placeholder="Login" id="Login" v-model="login_time" type="time" class="form-control" :class="{
                          'is-invalid':
                            AddAttendance_error &&
                            AddAttendance_error.login_time,}" />
                      <!--error handling-->
                      <div :class="{
                          'invalid-feedback':
                            AddAttendance_error &&
                            AddAttendance_error.login_time,
                        }" v-if="
                          AddAttendance_error && AddAttendance_error.login_time
                        ">
                        {{ AddAttendance_error.login_time[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6 position-relative form-group">
                    <label>Breakin Time</label>
                    <input placeholder="Breakin" id="Breakin" v-model="breakin_time" type="time" class="form-control" :class="{
                        'is-invalid': AddAttendance_error && AddAttendance_error.breakin_time, }" />
                    <div :class="{
                        'invalid-feedback':
                          AddAttendance_error &&
                          AddAttendance_error.breakin_time,
                      }" v-if="
                        AddAttendance_error && AddAttendance_error.breakin_time">
                      {{ AddAttendance_error.breakin_time[0] }}
                    </div>
                  </div>
                </div>

                <div class="form-row">
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Breakout Time</label>
                      <input name="Start-date" id="Breakout" v-model="breakout_time" placeholder="" type="time" class="form-control" :class="{
                          'is-invalid':
                            AddAttendance_error &&
                            AddAttendance_error.breakout_time,
                        }" />
                      <div :class="{
                          'invalid-feedback':
                            AddAttendance_error &&
                            AddAttendance_error.breakout_time,
                        }" v-if="
                          AddAttendance_error &&
                          AddAttendance_error.breakout_time ">
                        {{ AddAttendance_error.breakout_time[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6 position-relative form-group">
                    <label>Logout Time</label>
                    <input name="End-date" id="Logout" v-model="logout_time" placeholder="" type="time" class="form-control" :class="{
                        'is-invalid':
                          AddAttendance_error &&
                          AddAttendance_error.logout_time,
                      }" />
                    <div :class="{
                        'invalid-feedback':
                          AddAttendance_error &&
                          AddAttendance_error.logout_time,
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
                      <input placeholder="Remarks" id="remarks" v-model="remarks" type="text" class="form-control" :class="{
                          'is-invalid':
                            AddAttendance_error && AddAttendance_error.remarks, }" />
                      <!--error handling-->
                      <div :class="{
                          'invalid-feedback':
                            AddAttendance_error && AddAttendance_error.remarks,
                        }" v-if="
                          AddAttendance_error && AddAttendance_error.remarks
                        ">
                        {{ AddAttendance_error.remarks[0] }}
                      </div>
                    </div>
                  </div>
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class=""> Cost</label>
                      <input placeholder="Cost" id="cost" v-model="cost" type="number" class="form-control" :class="{
                          'is-invalid': AddAttendance_error && AddAttendance_error.cost, }" />
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
                      <button class="mt-2 float-right ml-2 btn btn-secondary" @click="CloseModal()">
                        Close
                      </button>
                      <button @submit.prevent="submitAddAttendance" class="mt-2 btn btn-success float-right">
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
    </div>
  </teleport>
</template>

<script>
import axios from "axios";
import Swal from "sweetalert2";
// import permissions from "../../../../../authorization/permissions";
export default {
  name: "AddAttendanceModal",
  props: ["employee_id"],
  data() {
    return {
      modalOpen: false,
      isModalVisible: false,
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
    this.employee = this.$props.employee_id.employee;
    this.isModalVisible = this.$props.employee_id.isModalVisible;
  },

  methods: {
    CloseModal() {
      this.isModalVisible = !this.isModalVisible;
      this.$emit("close-modal", this.isModalVisible);
    },

    async submitAddAttendance() {
      // const token = localStorage.getItem("token");
      axios
        .post("attendance-records/", {
          attendance_datetime: this.attendance_datetime,
          employee: this.employee,
          login_time: this.login_time,
          breakin_time: this.breakin_time,
          breakout_time: this.breakout_time,
          logout_time: this.logout_time,
          remarks: this.remarks,
          cost: this.cost,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text: "You have successfully Added Attendance ..",
          }).then((result) => {
            this.$router.push("attendance-records");
            this.$router.go();
            console.log(result);
          });
          console.log(response);
        })
        .catch((error) => {
          this.AddAttendance_error = error.response.data;
          console.log("--++", error.response);
        });
      // console.log(response);
    },

    // // Permissions
    // /*for Module(app)*/
    // hasModulePermission(...module_name) {
    //     return permissions.hasModulePermission(...module_name)
    //     // return permissions.hasModulePermission.apply(...module_name)
    // },
    //
    // /*for Model*/
    // hasModelPermission(model_name) {
    //     return permissions.hasModelPermission(model_name)
    // },
    //
    // /*for code_name*/
    // hasPermission(permission_name) {
    //     return permissions.hasPermission(permission_name)
    // },
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