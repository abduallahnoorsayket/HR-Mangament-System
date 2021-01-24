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
                            <li class="nav-item active">
                                <router-link class="nav-link" :to="{name: 'IssueList'}">
                                    <strong>Issues</strong>
                                </router-link>
                            </li>
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'AddIssue'}">
                                    <strong>Add New Issue</strong>
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
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'FilesList'}">
                                    Files
                                </router-link>
                            </li>

                        </ul>
                    </div>
                </nav>


                <div id="accordion" class="acordinContainer">
                    <div class="card">
                        <div id="headingOne" class="">
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="headerContaier">
                                        <div class="float-left">
                                            <h5>Advanced Search</h5>
                                        </div>
                                        <div class="float-right">
                                            <i id="addIcon" class="fa fa-chevron-circle-down" style="font: 45px"
                                               data-toggle="collapse" data-target="#collapseOne1" aria-expanded="true"
                                               aria-controls="collapseOne"></i>
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
                                            <label>Status </label>
                                            <input type="text" v-model="file_name" class="form-control"/>
                                        </div>
                                        <div class="col-md-6">
                                            <label> Priority</label>
                                            <input type="text" v-model="purpose" class="form-control"/>
                                        </div>
                                    </div>

                                    <div class="form-row">
                                        <div class="col-md-8"></div>
                                        <div class="col-md-4">
                                            <label></label>
                                            <br/>
                                            <br/>
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
                </div>


                <div class="row" v-if="issue_list">
                    <div class="col-12">
                        <div class="main-card card">
                            <h6 class="ml-4"> Project Issues</h6>
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-striped">
                                        <thead>
                                        <tr>
                                            <th scope="col">SL</th>
                                            <th scope="col">Subject</th>
                                            <th scope="col">Work Type</th>
                                            <th scope="col">Status</th>
                                            <th scope="col">Priority</th>
                                            <!--<th scope="col">Progress</th>-->
                                            <th scope="col">Created</th>
                                            <th scope="col">Action</th>
                                        </tr>
                                        </thead>

                                        <tbody>
                                        <tr v-for="(issue, SL) in issue_list.project_issue" :key="issue">
                                            <td> {{SL +1 }}</td>
                                            <td>{{ issue.subject }}</td>
                                            <td>{{ issue.work_type.name }}</td>
                                            <td>{{ issue.work_status.name }}</td>
                                            <td>{{ issue.work_priority.name }}</td>
                                            <td>{{ issue.created_datetime }}</td>



                                            <td class="td_action">
                                                <router-link class="item"
                                                             :to="{ name: 'IssueDetails', params: { id: issue.id } }">
                                                    <i class="fas fa-eye" style="color: #72e3da; padding: 10px"></i>
                                                </router-link>

                                                <router-link class="item"
                                                             :to="{ name: 'UpdateGroup', params: { id: issue_list.id }}">
                                                    <i class="fas fa-edit" style="color: #72e3da"></i>
                                                </router-link>
                                            </td>
                                        </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <br/>
            </div>
        </template>
    </Master>
</template>

<script>
    import axios from "axios";
    // import Swal from "sweetalert2";
    import Master from "../../../Master";

    export default {
        name: "IssueList",
        components: {Master},
        data() {
            return {
                issue_list: null
            };
        },


        async created() {
            await this.GetIssueList();
        },

        methods: {
            GetIssueList: function () {
                // axios.get("projects/").then((response) => {
                axios.get(`projects/${this.$route.params.id}/`, {}).then((response) => {
                    this.issue_list = response.data;
                    console.log(this.issue_list);
                    console.log(response.status);
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