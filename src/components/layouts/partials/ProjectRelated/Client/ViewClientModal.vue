<template>
  <div class="text-center mt-4"></div>
  <teleport to="body">
    <div v-if="isUpdateVisible" class="modal">
      <div class="main-card card">
        <div class="card-header">
          <p class="float-left">View Client Info</p>
        </div>
        <div class="card-body" v-if="form_data">
          <div class="row">
            <div class="col-md-12">
              <p><strong>Client Name:</strong>
                <br> {{form_data.client_name}}
              </p>
              <br>
              <p v-if="form_data.client_details">
                <strong>Client Details:</strong>
                <br>
                <span v-html="form_data.client_details"></span>
              </p>
              <p v-else>
                <br>
                <span> No details found !</span>
              </p>
              <button class="mt-2 float-right ml-2 btn btn-secondary" @click="CloseUpdateModal()">
                Close
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </teleport>
</template>

<script>
import axios from "axios";
export default {
  name: "ViewClientModal",
  props: ["employee_update"],
  data() {
    return {
      update_attendance_id: null,
      modalOpen: false,
      isModalVisible: false,
      isUpdateVisible: false,
      form_data: null,
    };
  },
  created() {
    this.update_attendance_id = this.$props.employee_update.update_attendance_id;
    this.isUpdateVisible = this.$props.employee_update.isUpdateVisible;
    this.getClientData();
  },
  methods: {
    CloseUpdateModal() {
      this.isUpdateVisible = !this.isUpdateVisible;
      this.$emit("close-update", this.isUpdateVisible);
    },
    getClientData: function () {
      axios.get("clients/" + this.update_attendance_id + "/").then(
        (response) => {
          this.form_data = response.data;
        },
        (response) => {
          console.log("----", response);
        }
      );
    },
  },
};
</script>
<style scoped>
/* teleport modal */
.main-card {
  width: 50%;
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