<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner" id="recordsModal">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Project Details</div>
            </div>
          </div>
        </div>
        <nav class="navbar navbar-expand-lg navbar navbar navbar-light" style="background-color: #e3f2fd;">
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item ">
                <router-link class="nav-link" :to="{name: 'ProjectDetails'}">
                  Project Overview
                </router-link>
              </li>
              <li class="nav-item ">
                <router-link class="nav-link" :to="{name: 'IssueList'}">
                  Issues
                </router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'AddIssue'}">
                  Add new Issue
                </router-link>
              </li>
              <li class="nav-item">
                <a class="nav-link " href="#">Activity</a>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" :to="{name: 'GanttChart'}">
                  Gantt Chart
                </router-link>
              </li>
              <li class="nav-item active">
                <router-link class="nav-link" :to="{name: 'FilesList'}">
                  <strong>Files</strong>
                </router-link>
              </li>
            </ul>
          </div>
        </nav>
        <!-- <div id="accordion" class="acordinContainer">
          <div class="card">
            <div id="headingOne" class="">
              <div class="row">
                <div class="col-md-12">
                  <div class="headerContaier">
                    <div class="float-left">
                      <h5>Advanced Search</h5>
                    </div>
                    <div class="float-right">
                      <i id="addIcon" class="fa fa-chevron-circle-down" style="font: 45px" data-toggle="collapse" data-target="#collapseOne1" aria-expanded="true" aria-controls="collapseOne"></i>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div data-parent="#accordion" id="collapseOne1" aria-labelledby="headingOne" class="collapse">
              <div class="card-body">
                <form action="" @submit.prevent="searchFile">
                  <div class="form-row">
                    <div class="col-md-6">
                      <label>File Name </label>
                      <input type="text" v-model="file_name" class="form-control" />
                    </div>
                    <div class="col-md-6">
                      <label>File Type </label>
                      <input type="text" v-model="purpose" class="form-control" />
                    </div>
                  </div>
                  <div class="form-row">
                    <div class="col-md-8"></div>
                    <div class="col-md-4">
                      <label></label>
                      <br />
                      <div class="form-group">
                        <button type="submit" class="btn btn-base float-right">
                          Search
                          <i class="fa fa-search"></i>&nbsp;
                        </button>
                      </div>
                    </div>
                  </div>
                </form>
              </div>
            </div>
          </div>
        </div> -->
        <div class="row mt-4" v-if="all_files">
          <div class="col-12">
            <div class="main-card card">
              <h6 class="ml-4"> Project Files</h6>
              <div class="card-body mt-4">
                <div class="table-responsive">
                  <table class="table table-striped">
                    <thead>
                      <tr>
                        <th scope="col">SL</th>
                        <th scope="col">Project / Issue</th>
                        <th scope="col">File name</th>
                        <th scope="col">Uploaded at </th>
                        <th scope="col">Uploaded by</th>
                        <th scope="col">Type</th>
                        <th scope="col" width="80px">Action</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr v-for="(file, index) in all_files" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{form_data.project_name}}</td>
                        <td>{{file.file_name}} </td>
                        <td>{{form_data.created_datetime}} </td>
                        <td>{{form_data.created_by.first_name}} {{ form_data.created_by.last_name }} </td>
                        <td v-if="file.content_type.model == 'project'"><span>Project</span></td>
                        <td v-else><span>Issue</span></td>
                        <td class="td_action">
                          <a :href="file.upload_files" class="btn btn-success">Download</a>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
        </div>
        <br />
        <!-- <div class="row">
          <div class="col-md-4">
            <ul class="pagination" v-if="pagination.count">
              <li class="page-item">
                <a href="#">Showing {{ pagination.showing }} of {{ pagination.count }}</a>
              </li>
              <li :class="{ disabled: !pagination.previous }" class="page-item">
                <a href="#!" v-on:click="setPage(pagination.previous)">Previous</a>
              </li>

              <li :class="{ disabled: !pagination.next }" class="page-item">
                <a href="#!" v-on:click="setPage(pagination.next)">Next</a>
              </li>
            </ul>
          </div>
          <div class="col-md-6"></div>
          <div class="col-md-2"></div>
        </div> -->
      </div>
    </template>
  </Master>
</template>

<script>
import axios from "axios";
import Master from "../../Master";

export default {
  name: "FilesList",
  components: { Master },
  data() {
    return {
      form_data:null,
      file_name: null,
      all_files: null,
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
    this.initialload();
    console.log("created");
  },
  updated() {
    this.initialload();
    console.log("updated");
  },
  methods: {
    initialload() {
      this.GetFilesList();
    },
    GetFilesList: function () {
      var queryParam = {
        file_name: this.$route.query.file_name,
        page: this.$route.query.page,
      };
      axios
        .get(`projects/${this.$route.params.id}/`, {
          params: queryParam,
        })
        .then((response) => {
          this.form_data = response.data;
          this.all_files = response.data.files;
          console.log("203===========", this.all_files);
          // this.pagination.count = response.data.count;
          // this.pagination.next = response.data.next;
          // this.pagination.previous = response.data.previous;
          // this.pagination.showing = response.data.results.length;
        });
    },
    setPage(page) {
      this.pagination.page = page;
      this.searchFile();
    },
    searchFile() {
      this.$router.push({
        path: "files-list",
        query: {
          file_name: this.file_name,
          page: this.pagination.page,
        },
      });
    },
  },
};
</script>

<style scoped>
.nav-item {
  margin: 0px 30px;
}
.headerContaier {
  margin: 0px 20px 40px 20px;
}

.card {
  margin-top: 20px;
  padding-top: 20px;
  margin-bottom: 20px;
}
</style>