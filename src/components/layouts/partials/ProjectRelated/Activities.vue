<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
              </div>
              <div>
                <div>Project Details</div>
              </div>
            </div>
          </div>
        </div>
        <nav class="navbar navbar-expand-lg navbar navbar navbar-light" style="background-color: #e3f2fd;" >
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
              <li class="nav-item active">
                <router-link class="nav-link" :to="{name: 'FilesList'}">
                  <strong>Activity</strong>
                </router-link>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Gannt Chart </a>
              </li>
              <li class="nav-item ">
                <router-link class="nav-link" :to="{name: 'FilesList'}">
                  <strong>Files</strong>
                </router-link>
              </li>
            </ul>
          </div>
        </nav>
           <ViewActivityModal @close-modal="CloseModal" v-if="isModalVisible" :employee_id="{
                    isModalVisible: isModalVisible,
                  }" />
        <!-- tree -->
        <div class="mt-4">
          <div class="activity-timeline-container">
            <div class="timeline">
              <div class="timeline-month">
                January </div>
              <div class="timeline-section">
                <div class="timeline-date">
                    2021-01-19
                </div>
                <div class="row">
                  <div class="col-sm-4">
                    <div class="timeline-box ">
                      <div class="box-title"><i class="fa fa-history" aria-hidden="true"></i> Test purpose</div>
                      <div class="box-content">
                        <a class="btn btn-xs btn-dark pull-right" data-toggle="modal" data-target="#activityModal813" @click="OpenModal()">Details</a>
                        <div class="box-item"><strong>Issue ID:</strong> 67945180</div>
                        <div class="box-item">
                          <strong>Spent hour:</strong>
                          10:00 Hours
                        </div>

                        <div class="box-item"><strong>Sub issue status:</strong> 30%</div>

                        <div class="row">
                          <div class="col-sm-6">
                            <div class="FinalIssue">
                              <strong>Final Issue status:</strong> 90%
                            </div>
                          </div>
                          <div class="col-sm-6">
                            <div class="progress progress-sm progress-striped active">
                              <div class="progress-bar progress-bar-success" data-toggle="tooltip" data-original-title="90%" style="width: 90%;"></div>
                            </div>
                          </div>
                        </div>
                      </div>
                      <div class="box-footer"><i class="fa fa-user" aria-hidden="true"></i> ABDULLAH SAYKET</div>
                    </div>
                  </div>

                  <!-- <div class="col-sm-4">
                    <div class="timeline-box">
                      <div class="box-title"><i class="fa fa-history" aria-hidden="true"></i> Test 2</div>
                      <div class="box-content">
                        <a class="btn btn-xs btn-dark pull-right" data-toggle="modal" data-target="#activityModal812">Details</a>
                        <div class="box-item"><strong>Issue ID:</strong> 67945180</div>
                        <div class="box-item">
                          <strong>Spent hour:</strong>
                          123:00 Hours
                        </div>

                        <div class="box-item"><strong>Sub issue status:</strong> 20%</div>

                        <div class="row">
                          <div class="col-sm-6">
                            <div class="FinalIssue">
                              <strong>Final Issue status:</strong> 60%
                            </div>
                          </div>
                          <div class="col-sm-6">
                            <div class="progress progress-sm progress-striped active">
                              <div class="progress-bar progress-bar-success" data-toggle="tooltip" data-original-title="60%" style="width: 60%;"></div>
                            </div>
                          </div>
                        </div>
                      </div>
                      <div class="box-footer"><i class="fa fa-user" aria-hidden="true"></i> MAKSUDUR RAHMAN MARUF</div>
                    </div>
                  </div> -->

                </div>
              </div>
              <!-- <div class="timeline-section">
                <div class="timeline-date">
                  <i class="fa fa-calendar" aria-hidden="true"></i>2021-01-18
                </div>
                <div class="row">
                  <div class="col-sm-4">
                    <div class="timeline-box">
                      <div class="box-title"><i class="fa fa-history" aria-hidden="true"></i> Test Sub Issue create</div>
                      <div class="box-content">
                        <a class="btn btn-dark pull-right">Details</a>
                        <div class="box-item"><strong>Issue ID:</strong> 67945180</div>
                        <div class="box-item">
                          <strong>Spent hour:</strong>
                          101:00 Hours
                        </div>

                        <div class="box-item"><strong>Sub issue status:</strong> 40%</div>

                        <div class="row">
                          <div class="col-sm-6">
                            <div class="FinalIssue">
                              <strong>Final Issue status:</strong> 40%
                            </div>
                          </div>
                          <div class="col-sm-6">
                            <div class="progress progress-sm progress-striped active">
                              <div class="progress-bar progress-bar-success" data-toggle="tooltip" data-original-title="40%" style="width: 40%;"></div>
                            </div>
                          </div>
                        </div>
                      </div>
                      <div class="box-footer"><i class="fa fa-user" aria-hidden="true"></i> MAKSUDUR RAHMAN MARUF</div>
                    </div>
                  </div>

                </div>
              </div> -->
            </div>
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import axios from "axios";
import Master from "../../Master";
// import Swal from "sweetalert2";
import ViewActivityModal from "./ViewActivityModal";
// import "../authheader";


export default {
  name: "Activities",
  components: { Master, ViewActivityModal },
  data() {
    return {
      isModalVisible: false,
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
    this.initialLoad();
    console.log("created");
  },
  updated() {
    this.initialLoad();
    console.log("updated");
  },

  methods: {
    initialLoad: function () {
      this.loadBranch();
    },
    OpenModal() {
      this.isModalVisible = !this.isModalVisible;
    },
    CloseModal(isModalVisible) {
      this.isModalVisible = isModalVisible;
    },
    

    loadBranch() {
      const token = localStorage.getItem("token");
      axios
        .get("branches/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.all_branchs = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },

    setPage(page) {
      this.pagination.page = page;
      this.searchUser();
    },
    searchUser() {
      console.log(this.searchName);
      console.log(this.searchMail);
      this.$router.push({
        path: "user-list",
        query: {
          first_name: this.first_name,
          username: this.username,
          department: this.department,
          groups: this.groups,
          skills: this.skills,
          page: this.pagination.page,
        },
      });
    },
  },
  // end of Methods
};
</script>
<style scoped>
/*@import url('https://fonts.googleapis.com/css?family=Montserrat|Roboto:300,400|Yellowtail');*/

@import url("https://fonts.googleapis.com/css?family=Varela+Round");

.vl {
  border: 1px solid #eeeeee;
}

.headerContaier {
  margin: 0px 20px 20px 20px;
}

.btn-base {
  background-color: #8be1db !important;
  color: white;
}

#addIcon {
  cursor: pointer;
}

.label {
  text-align: left;
}

/*table  design
    */

/* jjkljkljkjkjkjkjk */
.row {
  font-family: "Varela Round", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /*margin-top: 60px;*/
  margin-bottom: 10px;
  /* margin-top: 10px; */
}

.card {
  margin-top: 30px;
  padding-top: 30px;
  margin-bottom: 30px;
}

.card-content p {
  padding-bottom: 3px;
}
.nav-item {
  margin: 0px 30px;
}
hr {
  height: 1px;
  background-color: #dcdcdc;
  border: none;
  /* margin-bottom: 20px; */
  margin: 0px 10px;
}
/* Css for card tree */
.timeline {
  margin-top: 20px;
  position: relative;
}

.timeline {
  position: relative;
  margin: 0 0 30px 0;
  padding: 0;
  list-style: none;
}

.activity-timeline-container .timeline:before {
  height: calc(100% + 20px);
}
.timeline:before {
  position: absolute;
  content: "";
  width: 4px;
  height: calc(100% + 40px);
  background: rgb(138, 145, 150);
  background: -moz-linear-gradient(
    left,
    rgba(138, 145, 150, 1) 0%,
    rgba(122, 130, 136, 1) 60%,
    rgba(98, 105, 109, 1) 100%
  );
  background: #374850;
  background: #374850;
  filter: progid: DXImageTransform.Microsoft.gradient( startColorstr='#8a9196', endColorstr='#62696d', GradientType=1);
  left: 14px;
  top: 5px;
  border-radius: 4px;
}

.timeline:before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;
  width: 4px;
  /* background: blue; */
  left: 31px;
  margin: 0;
  border-radius: 2px;
}
.timeline-month {
  color: black;
  position: relative;
  padding: 4px 15px 4px 35px;
  background-color: #e3f2fd;;
  display: inline-block;
  width: auto;
  border-radius: 40px;
  border: 1px solid #00c0ef;
  border-right-color: #00c0ef;
  margin-bottom: 30px;
  font-weight: 700;
}

.timeline-month:before {
  position: absolute;
  content: "";
  width: 20px;
  height: 20px;
  background: #374850;
  border-radius: 100%;
  left: 5px;
}

.timeline-section {
  padding-left: 55px;
  display: block;
  position: relative;
  margin-bottom: 30px;
}
.timeline-section:before {
  content: "";
  position: absolute;
  width: 30px;
  height: 1px;
  background-color: #007963;
  top: 12px;
  left: 30px;
}
.timeline-date {
  margin-bottom: 15px;
  padding: 2px 15px;
  background: #374850;
  position: relative;
  display: inline-block;
  border-radius: 20px;
  border: 1px solid #374850;
  color: #fff;
  text-shadow: 1px 1px 1px rgba(0, 0, 0, 0.3);
}
/*  custom row */
.row {
  margin-right: -15px;
  margin-left: -15px;
}

.row:before {
  display: table;
  content: " ";
}

.timeline-section .col-sm-4 {
  margin-bottom: 15px;
}

.timeline-box {
  position: relative;
  /* background-color: #374850; */
  border-radius: 15px;
  /* border-top-left-radius: 0px;
  border-bottom-right-radius: 0px; */
  /* border: 1px solid #374850; */
  -webkit-transition: all 0.3s ease;
  -o-transition: all 0.3s ease;
  transition: all 0.3s ease;
  overflow: hidden;
   border: 1px solid #374850;
  /* box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2); */
}

.timeline-box .box-title {
  background: #374850;
  color: #fff;
  padding: 10px;
  font-weight: 600;
}
.timeline-box .box-title i {
  color: #b8c7ce;
}

.timeline-box .box-content {
  background: #fff;
  padding: 12px;
  text-align: left;
}

.timeline-box .box-footer {
  text-align: right;
  background: #fafafa;
  padding: 8px 12px;
}

.box-footer {
  border-top-left-radius: 0;
  border-top-right-radius: 0;
  border-bottom-right-radius: 3px;
  border-bottom-left-radius: 3px;
  border-top: 1px solid #f4f4f4;
  padding: 10px;
  background-color: #fff;
}
.FinalIssue {
  text-align: left;
}
a:not([href]):not([tabindex]) {
  color: white;
  text-decoration: none;
}
/*  custom css  */
/* .calenderIcon{
  margin-right:px;
} */
</style>
