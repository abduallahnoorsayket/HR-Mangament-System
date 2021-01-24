<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="pe-7s-add-user icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Add Client</div>
            </div>
          </div>
        </div>
        <div class="main-card card">
          <div class="card-body">
            <form @submit.prevent="submitClientForm">
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-12">
                    <div class="position-relative form-group">
                      <label>Client Name</label>
                      <input placeholder="name" v-model="client_name" type="text" class="form-control" :class="{
                          'is-invalid':
                            department_error_data && department_error_data.client_name,}" />
                      <!--error handling-->
                      <div :class="{
                          'invalid-feedback':
                            department_error_data && department_error_data.client_name,
                        }" v-if="
                          department_error_data && department_error_data.client_name">
                        {{ department_error_data.client_name[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-12 position-relative form-group">
                    <label>Client Details</label>
                    <ckeditor :editor="editor" placeholder="Details" v-model="client_details" rows="5" :config="editorConfig" ></ckeditor>
                    <div v-if="department_error_data && department_error_data.client_details" class="alert alert-danger alert-dismissible fade show text-left mt-1" role="alert">
                     {{ department_error_data.client_details[0] }}
                      <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <!--error handling-->
                  </div>
                </div>
                <div class="form-group">
                  <button @submit.prevent="submitClientForm" class="mt-2 btn btn-success btn-lg float-right">
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
import ClassicEditor from "@ckeditor/ckeditor5-build-classic";
import CKEditor from "@ckeditor/ckeditor5-vue";

export default {
  name: "CreateRating",
  components: { Master, ckeditor: CKEditor.component },
  data() {
    return {
      editor: ClassicEditor,
      client_name: null,
      client_details: null,
      department_error_data: {
        client_name: null,
        client_details: null,
      },
    };
  },
  methods: {
    async submitClientForm() {
      const token = localStorage.getItem("token");
      const response = await axios
        .post("clients/", {
          headers: {
            Authorization: `token ${token}`,
          },
          client_name: this.client_name,
          client_details: this.client_details,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text: "You have successfully Added client.",
          }).then((result) => {
            this.$router.push("client-list");
            console.log(result);
          });
          console.log(response);
        })
        .catch((error) => {
          this.department_error_data = error.response.data;
          console.log("--++", error.response);
        });
      console.log(response);
    },
  },
};
</script>
<style>
  .ck-editor__editable {
    min-height: 380px;
   }
</style>