<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-edit icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Edit Rating</div>
            </div>
          </div>
        </div>

        <div class="main-card card">
          <div class="card-body">
            <form @submit.prevent="EditRating">
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Rating Name</label>
                      <input placeholder="name" v-model="name" type="text" class="form-control" :class="{
                          'is-invalid':
                            department_error_data && department_error_data.name,}" />
                      <!--error handling-->
                      <div :class="{
                          'invalid-feedback':
                            department_error_data && department_error_data.name,
                        }" v-if="
                          department_error_data && department_error_data.name ">
                        {{ department_error_data.name[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6 position-relative form-group">
                    <label>Rating Status</label>
                    <select name="select" v-model="status" type="choice" class="form-control" :class="{
                        'is-invalid':
                          department_error_data && department_error_data.status, }">
                      <option value="" selected>Select Status</option>
                      <option value="true">ON</option>
                      <option value="false">OFF</option>
                    </select>
                    <!--error handling-->
                    <div :class="{
                        'invalid-feedback':
                          department_error_data && department_error_data.status,
                      }" v-if="
                        department_error_data && department_error_data.status ">
                      {{ department_error_data.status[0] }}
                    </div>
                  </div>
                </div>

                <div class="form-group">
                  <button @submit.prevent="EditRating" class="mt-2 btn btn-success btn-lg float-right">
                    Submit
                  </button>
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
import Master from "../../../Master";
import axios from "axios";
import Swal from "sweetalert2";
import "../../../authheader";
export default {
  name: "EditRating",
  components: { Master },
  data() {
    return {
      name: null,
      status: null,

      department_error_data: {
        name: null,
        status: null,
      },
    };
  },
  methods: {
    getRating: function () {
      axios.get(`ratings/${this.$route.params.id}/`).then(
        (response) => {
          this.name = response.data.name;
          this.status = response.data.status;
        },
        (response) => {
          console.log("----", response);
        }
      );
    },

    EditRating() {
      const token = localStorage.getItem("token");
      axios
        .put("ratings/" + this.$route.params.id + "/", {
          headers: {
            Authorization: `token ${token}`,
          },
          name: this.name,
          status: this.status,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text: "You have successfully edited.",
          }).then((response) => {
            this.$router.push({ name: "RatingList" });
            console.log(response);
          });
          console.log(response);
        })
        .catch((error) => {
          this.department_error_data = error.response.data;
          console.log("--++", error.response);
        });
    },
  },
  created() {
    this.getRating();
  },
};
</script>

<style scoped>
</style>