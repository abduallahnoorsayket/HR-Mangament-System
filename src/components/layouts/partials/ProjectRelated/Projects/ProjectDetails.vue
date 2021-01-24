<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner" id="recordsModal" v-if="form_data">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Project Details</div>
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
        <nav class="navbar navbar-expand-lg navbar navbar navbar-light" style="background-color: #e3f2fd;">
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item active">
                <router-link class="nav-link" :to="{name: 'ProjectDetails', params: { id: form_data.id } }">
                  <strong>Project Overview</strong>
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'IssueList', params: { id: form_data.id } }">
                  Issues
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'AddIssue'}">
                  Add New Issue
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'Activities',params: { id: form_data.id } }">
                  Activity
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'GanttChart'}">
                 Gantt Chart
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'FilesList' , params: { id: form_data.id }}">
                  Files
                </router-link>
              </li>
            </ul>
          </div>
        </nav>
        <div class="row mt-2">
          <div class="col-md-9">
            <div class="main-card card">
              <div class="detailContaier">
                <h5>Project Overview
                  <router-link class="item" :to="{ name: 'UpdateNewProject', params: { id: form_data.id } }">
                    <i class="fas fa-edit mr-2 float-right" style="color: #72e3da;  font-size: 20px "></i>
                  </router-link>
                </h5>
                <hr>
                <br>
                <h2> {{form_data.project_name}}</h2>
                <br>
                <br>
                <p v-if="form_data.description">
                  <span v-html="form_data.description"></span>
                </p>
                <br>
                <br>
                <strong class="mb-2">Project Files:</strong>
                <br>
                <template v-for="(file, index) in form_data.files" :key="index">
                  <a :href="file.upload_files" class="btn btn-info ml-1">{{file.file_name}}</a>
                </template>
              </div>
            </div>
          </div>
          <div class="col-md-3">
            <div class="main-card card">
              <div class="infoContaier">
                <h5>Project Information</h5>
                <hr>
                <p class="projectInfo"><strong>Branch: </strong><span>{{form_data.branch.branch_name}}</span></p>
                <p class="projectInfo"><strong>Client: </strong><span>{{form_data.client.client_name}}</span></p>
                <p class="projectInfo"><strong>Date: </strong><span>{{form_data.date}}</span></p>
                <p class="projectInfo"><strong>Status: </strong><span>{{form_data.status}}</span></p>

                <h5 class="mt-4"> Assignee Employees </h5>
                <hr>
                <p>
                  <strong class="mr-1"> Project Manager:</strong>
                  <br>
                  <template v-for="(manager, index) in form_data.managers" :key="index">
                    {{manager.first_name + ' '+manager.last_name}}
                  </template>
                </p>
                <p>
                  <strong class="mr-1">Developers:</strong>
                  <br>
                  <template v-for="(developer, index) in form_data.developers" :key="index">
                    {{developer.first_name + ' '+developer.last_name}}<br>
                  </template>
                </p>
                <p>
                  <strong class="mr-1">Tester:</strong>
                  <br>
                  <template v-for="(tester, index) in form_data.testers" :key="index">
                    {{tester.first_name + ' '+tester.last_name}}
                  </template>
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import axios from "axios";
// import Swal from "sweetalert2";
import Master from "../../../Master";

export default {
  name: "ProjectDetails",
  components: { Master },
  data() {
    return {
      form_data: null,
      isUpdateVisible: false,
      update_attendance_id: null,
      searchName: null,
      all_clients: null,
      pagination: {
        count: null,
        next: null,
        previous: null,
        showing: 0,
        page: null,
      },
    };
  },
  created() {
    this.getProjectDeatails();
  },
  methods: {
    getProjectDeatails: function () {
      axios.get(`projects/${this.$route.params.id}/`).then(
        (response) => {
          this.form_data = response.data;
          // this.form_data.leave_type = this.form_data.leave_type.id;
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
.detailContaier {
  margin: 2%;
}

.infoContaier {
  margin: 18px;
}

.nav-item {
  margin: 0px 30px;
}
.projectInfo {
  margin-bottom: 8px;
}
</style>