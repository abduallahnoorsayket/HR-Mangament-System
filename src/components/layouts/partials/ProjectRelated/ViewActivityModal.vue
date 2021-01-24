<template>
  <div class="mt-2"></div>
  <teleport to="body">
    <div v-if="isModalVisible" class="modal">
      <div>
        <div class="main-card card">
          <div class="card-header">
            <p class="float-left">View Activity Info</p>
          </div>
          <div class="card-body">
            <div class="row">
              <div class="col-md-12">
                <div class="subissue-modal">
                  <div class="row">
                    <div class="col-md-12 mb-7">
                      <strong>Parent issue name:</strong> Attendance Module Frontend
                    </div>
                    <div class="col-md-12 mb-7">
                      <strong>Parent issue ID:</strong> #67945180
                    </div>
                  </div>
                  <div class="row mb-7">
                    <div class="col-md-4">
                      <strong>Created by:</strong> ABDULLAH SAYKET
                    </div>
                    <div class="col-md-4">
                      <strong>Priority:</strong> Normal
                    </div>
                    <div class="col-md-4">
                      <strong>Work type:</strong> Module Development
                    </div>
                  </div>
                  <div class="row mb-30">
                    <div class="col-md-4">
                      <strong>Issue previous done:</strong> 60%
                    </div>
                    <div class="col-md-4">
                      <strong>Issue finishing done:</strong> 90%
                    </div>
                    <div class="col-md-4 mb-7">
                      <strong>Sub issue done:</strong> 30%
                    </div>
                    <div class="col-md-4 mb-7">
                      <strong>Spent hour:</strong> 10:00 Hours
                    </div>
                    <div class="col-sm-8">
                      <div class="progress progress-sm progress-striped active mt-5">
                        <div class="progress-bar progress-bar-success" data-toggle="tooltip" data-original-title="90%" style="width: 90%;"></div>
                      </div>
                    </div>
                  </div>
                  <div class="row">
                    <div class="col-md-12">
                      <strong>Work Description::</strong>
                      <div>
                        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed vel consequat dui. Nunc et nunc eget risus egestas vestibulum ac sit amet diam. Sed at enim ut diam ultrices dapibus. Ut pharetra rutrum dolor vitae vulputate. Etiam venenatis lacus nec felis hendrerit, vel pulvinar sapien porta. Proin quis ligula non nibh posuere feugiat. Phasellus pulvinar augue at diam consequat, nec pulvinar enim venenatis.</p>
                      </div>
                    </div>
                  </div>
                </div>
                <button class="mt-2 float-right ml-2 btn btn-secondary" @click="CloseModal()">
                  Close
                </button>
              </div>
            </div>
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
  name: "ViewActivityModal",
  props: ["employee_id"],
  data() {
    return {
      modalOpen: false,
      isModalVisible: false,
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
  },
};
</script>
<style scoped>
/* teleport modal */
/* teleport modal */
.main-card {
  width: 1000px;
}

.modal {
  /* position: absolute; */
  top: 20;
  right: 0;
  bottom: 20;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  flex-direction: column;
  align-items: center !important;
  justify-content: center !important;
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