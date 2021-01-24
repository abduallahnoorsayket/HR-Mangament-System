<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-marker icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Update Project</div>
              <router-link :to="{ path: '/project-list' }">
                <a class="float" style="position: absolute; right: 0">
                  <button type="button" class="btn btn-success" style="font-size: 15px">
                    <span class="fas fa-list"></span> Project List
                  </button>
                </a>
              </router-link>
            </div>
          </div>
        </div>

        <div class="row">
          <div class="col-12">
            <div class="main-card card">
              <div class="card-body">
                <form @submit.prevent="editProjectForm">
                  <div class="form-group">

                    <!--Project Name-->
                    <div class="form-row">
                      <div class="col-md-12">
                        <div class="position-relative form-group">
                          <label>Project Name <span class="asterisk">*</span></label>
                          <input placeholder="project name" id="project_name" v-model="project_name" type="text" class="form-control" :class="{ 'is-invalid': error_data && error_data.project_name }" />

                          <!--error handling-->
                          <div :class="{ 'invalid-feedback': error_data && error_data.project_name }" v-if="error_data && error_data.project_name">
                            {{ error_data.project_name[0] }}
                          </div>
                        </div>
                      </div>
                    </div>

                    <!--Description-->
                    <div class="form-row">
                      <div class="col-md-10">
                        <div class="position-relative form-group">
                          <label> Description <span class="asterisk">*</span></label>
                          <br />
                          <ckeditor :editor="editor" v-model="description" :config="editorConfig"></ckeditor>
                          <div v-if="error_data && error_data.description" class="alert alert-danger alert-dismissible fade show text-left mt-1" role="alert">
                            {{ error_data.description[0] }}
                            <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                              <span aria-hidden="true">&times;</span>
                            </button>
                          </div>
                        </div>
                      </div>
                      <div class="col-md-2">
                        <div class="position-relative form-group">
                          <div class="instruction">
                            <h6 class="ml-4"><b>Instructions:</b></h6>
                            <ul>
                              <li>Please write project detail information here.</li>
                              <li>Add project duration.</li>
                              <li>Add project starting date and ending date.</li>
                            </ul>
                          </div>
                        </div>
                      </div>
                    </div>

                    <!--Project URL-->
                    <div class="form-row">
                      <div class="col-md-12">
                        <div class="position-relative form-group">
                          <label>Project URL <span class="asterisk">*</span></label>
                          <input placeholder="project url" v-model="project_url" type="text" class="form-control" :class="{ 'is-invalid': error_data && error_data.project_url }" />

                          <!--error handling-->
                          <div :class="{ 'invalid-feedback': error_data && error_data.project_url }" v-if="error_data && error_data.project_url">
                            {{ error_data.project_url[0] }}
                          </div>
                        </div>
                      </div>
                    </div>

                    <!--Upload File-->
                    <div class="form-row">
                      <div class="col-md-12">
                        <div class="position-relative form-group mt-4">

                          <!-- new file -->
                          <template v-for="(file, k) in files_related" :key="k">
                            <div class="row mt-4">
                              <div class="col-md-5">
                                <label> Upload Document: <span class="asterisk">*</span></label>
                                <input type="file" class="form-control" @change="handleImage($event,k)" id="file" />
                                <input type="hidden" v-model="file.upload_files" id="prevFile" />
                              </div>

                              <div class="col-md-5">
                                <label>Document Name: <span class="asterisk">*</span></label>
                                <input class="form-control" type="text" autocomplete="off" v-model="file.file_name" placeholder="">
                              </div>
                              <div class="col-md-2 mt-4">
                                <button class="btn btn-sm btn-del remove mt-1" style="color: firebrick; font-size: 20px; "> <i class="fas fa-trash-alt " @click="deleteRow(k)"></i> </button>
                              </div>
                            </div>
                            <div v-if="error_data && error_data.non_field_errors" class="col-md-10 alert alert-danger alert-dismissible fade show text-center " role="alert">
                              These fields may not be blank
                              <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                              </button>
                            </div>
                          </template>
                        </div>
                        <button type='button' class="btn btn-info" @click="addNewRow">
                          <i class="fas fa-plus-circle"></i>
                          ADD NEW
                        </button>
                      </div>
                    </div>
                    <!--Branch & Client-->
                    <div class="form-row mt-4">
                      <!--Branch-->
                      <div class="col-md-6">
                        <div class="position-relative form-group">
                          <label>Branch <span class="asterisk">*</span></label>
                          <select name="select[]" v-model="branch" class="form-control" :class="{ 'is-invalid': error_data && error_data.branch }">

                            <option value="false" disabled selected>Select Branch</option>
                            <option v-for="branches in all_branch" :key="branches.id" :value="branches.id">
                              {{ branches.branch_name }}
                            </option>
                          </select>
                          <div :class="{ 'invalid-feedback': error_data && error_data.branch }" v-if="error_data && error_data.branch">
                            {{ error_data.branch[0] }}
                          </div>
                        </div>
                      </div>

                      <!--Client-->
                      <div class="col-md-6">
                        <div class="position-relative form-group">
                          <label>Client <span class="asterisk">*</span></label>
                          <select name="select[]" v-model="client" class="form-control" :class="{ 'is-invalid': error_data && error_data.client }">

                            <option value="false" disabled selected>Select Client</option>
                            <option v-for="client in all_clients" :key="client.id" :value="client.id">
                              {{ client.client_name }}
                            </option>
                          </select>
                          <div :class="{ 'invalid-feedback': error_data && error_data.client }" v-if="error_data && error_data.client">
                            {{ error_data.client[0] }}
                          </div>
                        </div>
                      </div>
                    </div>

                    <!--ADMINS, MANAGERS, DEVELOPERS, TESTERS-->
                    <div class="form-row mt-4">
                      <!--ADMINS LIST-->
                      <div class="col-md-3 permissions">
                        <label>Admin</label><br>
                        <input type="text" class="form-control" v-model="searchAdmin" placeholder="filter" style="outline: none;">
                        <div class=" grp-per" v-if="all_users">

                          <div class="position-relative form-check" v-for="admin in resultAdmin" :key="admin.id">
                            <label class="form-check-label">
                              <input type="checkbox" :id="admins" :value="admin.id" v-model="admins" class="form-check-input" />
                              {{ admin.first_name + ' ' + admin.last_name}}
                            </label>

                          </div>
                        </div>
                      </div>

                      <!--MANAGERS LIST-->
                      <div class="col-md-3 permissions">
                        <label>Managers</label><br>
                        <input type="text" class="form-control" v-model="searchManager" placeholder="filter" style="outline: none;">
                        <div class=" grp-per" v-if="all_users">

                          <div class="position-relative form-check" v-for="manager in resultManager" :key="manager.id">
                            <label class="form-check-label">
                              <input type="checkbox" :value="manager.id" v-model="managers" class="form-check-input" />
                              {{ manager.first_name + ' ' + manager.last_name}}
                            </label>

                          </div>
                        </div>
                      </div>

                      <!--DEVELOPERS LIST-->
                      <div class="col-md-3 permissions">
                        <label>Developers</label><br>
                        <input type="text" class="form-control" v-model="searchDeveloper" placeholder="filter" style="outline: none;">
                        <div class=" grp-per" v-if="all_users">

                          <div class="position-relative form-check" v-for="developer in resultDeveloper" :key="developer.id">
                            <label class="form-check-label">
                              <input type="checkbox" :value="developer.id" v-model="developers" class="form-check-input" />
                              {{ developer.first_name + ' ' + developer.last_name}}
                            </label>

                          </div>
                        </div>
                      </div>

                      <!--TESTERS LIST-->
                      <div class="col-md-3 permissions">
                        <label>Testers</label><br>
                        <input type="text" class="form-control" v-model="searchTester" placeholder="filter" style="outline: none;">
                        <div class=" grp-per" v-if="all_users">

                          <div class="position-relative form-check" v-for="tester in resultTester" :key="tester.id">
                            <label class="form-check-label">
                              <input type="checkbox" :value="tester.id" v-model="testers" class="form-check-input" />
                              {{ tester.first_name + ' ' + tester.last_name}}
                            </label>

                          </div>
                        </div>
                      </div>
                    </div>

                    <!--Status-->
                    <div class="form-row mt-2">
                      <div class="col-md-12">
                        <label>Status <span class="asterisk">*</span></label>
                        <select name="select" id="type" v-model="status" type="choice" class="form-control" :class="{ 'is-invalid': error_data && error_data.status }">
                          <option value="PN">Pending</option>
                          <option value="RN">Running</option>
                          <option value="CM">Completed</option>
                          <option value="HO">Handover</option>
                          <option value="PP">Postponed</option>
                        </select>
                        <div :class="{ 'invalid-feedback': error_data && error_data.status }" v-if="error_data && error_data.status">
                          {{ error_data.status[0] }}
                        </div>
                      </div>

                     
                    </div>

                    <div class="form-group mt-2">
                      <button @submit.prevent="editProjectForm" class="mt-2 btn btn-success btn-lg float-right">Update Project
                      </button>
                    </div>
                  </div>
                </form>
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
import axios from "axios";
import ClassicEditor from "@ckeditor/ckeditor5-build-classic";
import CKEditor from "@ckeditor/ckeditor5-vue";
import Swal from "sweetalert2";
// import $ from "jquery";

export default {
  name: "UpdateNewProject",
  components: {
    Master,
    ckeditor: CKEditor.component,
  },
  data() {
    return {
      newdataURL: null,
      content_id: null,
      project_name: null,
      description: null,
      project_url: null,
      branch: null,
      client: null,
      admins: [],
      managers: [],
      developers: [],
      testers: [],
      status: null,
      created_datetime: null,
      all_branch: null,
      all_clients: null,
      all_users: null,
      
      searchAdmin: null,
      searchManager: null,
      searchDeveloper: null,
      searchTester: null,
      file_id: null,
      files_related: [],
      file_obj: {
        // file_name: "",
        // upload_files: "",
        // content_type: "",
      },
      error_data: {
        project_name: null,
        description: null,
        project_url: null,
        branch: null,
        client: null,
        // admins: null,
        // managers: null,
        // developers: null,
        // testers: null,
        status: null,
        created_datetime: null,
        files: null,
      },

      // for CKEditor
      editor: ClassicEditor,
    };
  },

  methods: {
    // Load branch into dropdown
    loadBranch() {
      const token = localStorage.getItem("token");
      axios
        .get("branches/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_branch = response.data;
          // this.pushReuseForm()
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    // Load Clients into dropdown
    loadClients() {
      const token = localStorage.getItem("token");
      axios
        .get("clients-list/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_clients = response.data;
          // this.pushReuseForm()
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    /*load User*/
    loadUsers() {
      const token = localStorage.getItem("token");
      axios
        .get("users/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_users = response.data.results;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    /*load Content types*/
    loadContentType() {
      const token = localStorage.getItem("token");
      axios
        .get("content-types/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_content = response.data;
          this.all_content.map((element) => {
            if (element.model === "project") {
              this.content_id = element.id;
            }
          });
          // this.file_obj.content_type = filted_content[0].id;
          // this.addNewRow();
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    /*Get Project Data*/
    getProjectData: function () {
      axios.get(`projects/${this.$route.params.id}/`).then(
        (response) => {
          this.project_name = response.data.project_name;
          this.description = response.data.description;
          this.project_url = response.data.project_url;
          this.branch = response.data.branch.id;
          this.client = response.data.client.id;
          this.admins = response.data.admins.map((admins) => admins.id);
          this.managers = response.data.managers.map((manager) => manager.id);
          this.developers = response.data.developers.map((dev) => dev.id);
          this.testers = response.data.testers.map((tester) => tester.id);
          // let vvm = this;
          response.data.files.forEach((element) => {
            let vm = this;
            this.toDataURL(element.upload_files, function (dataUrl) {
              console.log("RESULT:///////////////", dataUrl);
              // vm.newdataURL = dataUrl;
              vm.files_related.push({
                id: element.id,
                file_name: element.file_name,
                upload_files: dataUrl,
                content_type: element.content_type.id,
              });
            });
          });

          this.status = response.data.status;
          this.created_datetime = response.data.created_datetime;
          console.log(" Files related------", this.files_related);
        },
        (response) => {
          console.log("----", response);
        }
      );
    },

    /*Submit Project Form*/
    async editProjectForm() {
      const token = localStorage.getItem("token");
      const response = await axios
        .put("projects/" + this.$route.params.id + "/", {
          headers: {
            Authorization: `token ${token}`,
          },
          project_name: this.project_name,
          description: this.description,
          project_url: this.project_url,
          branch: this.branch,
          client: this.client,
          admins: this.admins,
          managers: this.managers,
          developers: this.developers,
          testers: this.testers,
          status: this.status,
          created_datetime: this.created_datetime,
          files: this.files_related,
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            text: "You have successfully edit a Project..",
          }).then((result) => {
            this.$router.push({ name: "ProjectList" });
            console.log(result);
          });
          console.log(response);
        })
        .catch((error) => {
          this.error_data = error.response.data;
          console.log("--++", error.response);
        });
      console.log(response);
    },
    // new row adding
    addNewRow() {
      this.files_related.push({
        file_name: "",
        upload_files: "",
        content_type: this.content_id,
      });
    },

    // Remove Row
    deleteRow(index) {
      if (index > -1) {
        this.files_related.splice(index, 1);
      }
    },

    /*for file upload*/
    handleImage(e, index) {
      let files = e.target.files || e.dataTransfer.files;
      if (!files.length) return;
      this.getBase64(files[0], index);
      console.log("493  zero ////////", files[0], index);
    },
    getBase64(file, index) {
      var reader = new FileReader();
      reader.readAsDataURL(file);
      var vm = this;
      reader.onload = function () {
        console.log(reader.result);
        vm.files_related[index].upload_files = reader.result;
      };
      reader.onerror = function (error) {
        console.log("Error: ", error);
      };
    },

    // file url to base64
    toDataURL(url, callback) {
      var xhr = new XMLHttpRequest();
      xhr.onload = function () {
        var reader = new FileReader();
        reader.onloadend = function () {
          callback(reader.result);
        };
        reader.readAsDataURL(xhr.response);
      };
      xhr.open("GET", url);
      xhr.responseType = "blob";
      xhr.send();
    },

    // this.toDataURL('https://www.gravatar.com/avatar/d50c83cc0c6523b4d3f6085295c953e0', function(dataUrl) {
    //   console.log('RESULT:', dataUrl)
    // })
  },

  created() {
    this.loadBranch();
    this.loadClients();
    this.loadUsers();
    this.getProjectData();
    this.loadContentType();
  },

  computed: {
    resultAdmin() {
      if (this.searchAdmin) {
        return this.all_users.filter((item) => {
          return this.searchAdmin
            .toLowerCase()
            .split(" ")
            .every((v) => item.first_name.toLowerCase().includes(v));
        });
      } else {
        return this.all_users;
      }
    },
    resultManager() {
      if (this.searchManager) {
        return this.all_users.filter((item) => {
          return this.searchManager
            .toLowerCase()
            .split(" ")
            .every((v) => item.first_name.toLowerCase().includes(v));
        });
      } else {
        return this.all_users;
      }
    },
    resultDeveloper() {
      if (this.searchDeveloper) {
        return this.all_users.filter((item) => {
          return this.searchDeveloper
            .toLowerCase()
            .split(" ")
            .every((v) => item.first_name.toLowerCase().includes(v));
        });
      } else {
        return this.all_users;
      }
    },
    resultTester() {
      if (this.searchTester) {
        return this.all_users.filter((item) => {
          return this.searchTester
            .toLowerCase()
            .split(" ")
            .every((v) => item.first_name.toLowerCase().includes(v));
        });
      } else {
        return this.all_users;
      }
    },
  },
};
// $(document).ready(function () {
//   $(".custom-file-input").on("change", function () {
//     var fileName = $(this).val().split("\\").pop();
//     $(this).siblings(".custom-file-label").addClass("selected").html(fileName);
//   });
// });
</script>

<style scoped>
.grp-per {
  margin-top: 15px;
  height: 150px;
  /*overflow: scroll;*/
  overflow-y: auto;
}
.asterisk {
  color: red;
}
.instruction {
  margin-top: 15%;
}
.form-group input {
  box-shadow: none !important;
}
</style>
