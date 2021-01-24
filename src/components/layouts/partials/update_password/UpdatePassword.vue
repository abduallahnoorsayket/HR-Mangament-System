<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-user-plus icon-gradient bg-tempting-azure"></i>
              </div>
              <div>
                Update Password
                <div class="page-title-subheading"></div>
              </div>
            </div>
          </div>
        </div>

        <div class="main-card card">
          <div class="card-body">
            <h5 class="card-title">Update Password</h5>

            <form @submit.prevent="submitUserForm">
              <div class="form-group">
                <div class="form-row">
                  <!--old-->
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Old Password</label>
                      <input
                        placeholder="Old Password"
                        id="old_password"
                        v-model="old_password"
                        type="password"
                        class="form-control"
                        :class="{
                          'is-invalid': error_data && error_data.old_password,
                        }"
                      />
                      <div
                        :class="{
                          'invalid-feedback':
                            error_data && error_data.old_password,
                        }"
                        v-if="error_data && error_data.old_password"
                      >
                        {{ error_data.old_password[0] }}
                      </div>
                    </div>
                  </div>

                  <!--new-->
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">New Password</label>
                      <input
                        placeholder="New Password"
                        id="new_password"
                        v-model="new_password"
                        type="password"
                        class="form-control"
                        :class="{
                          'is-invalid': error_data && error_data.new_password,
                        }"
                      />
                      <div
                        :class="{
                          'invalid-feedback':
                            error_data && error_data.new_password,
                        }"
                        v-if="error_data && error_data.new_password"
                      >
                        {{ error_data.new_password[0] }}
                      </div>
                    </div>
                  </div>
                </div>

                <div class="form-group">
                  <button class="mt-2 btn btn-primary btn-lg float-right">
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
import Master from "@/components/layouts/Master";
import axios from "axios";
import Swal from "sweetalert2";

export default {
  name: "UpdatePassword",
  components: { Master },
  data() {
    return {
      old_password: null,
      new_password: null,
      error_data: null,
    };
  },
  methods: {
    submitUserForm() {
      const token = localStorage.getItem("token");
      Swal.fire({
        title: "Are you sure?",
        text: "You password will  be cahnged!",
        icon: "warning",
        showCancelButton: true,
        confirmButtonColor: "#3085d6",
        cancelButtonColor: "#d33",
        confirmButtonText: "Yes, change it!",
      }).then((result) => {
        if (result.isConfirmed) {
          axios
            .put("update-password/", {
              headers: { Authorization: `token ${token}` },
              old_password: this.old_password,
              new_password: this.new_password,
            })
            .then((response) => {
              if (response.status === 204) {
                Swal.fire(
                  "changed!",
                  "Your password  has been Updated.",
                  "success"
                );
                this.$router.push("/user-profile");
              }
            })
            .catch((error) => {
              this.error_data = error.response.data;
            });
        } else {
          Swal.fire("Cancelled", "Your pass has not been Updated !", "error");
        }
      });
      // this.$router.push("/user-profile");
    },
    // end of submit
  },
  //   end of methodss
};
</script>

<style scoped>
.vl {
  border: 1px solid #eeeeee;
}

.grp-per {
  height: 135px;
  overflow: scroll;
}
</style>