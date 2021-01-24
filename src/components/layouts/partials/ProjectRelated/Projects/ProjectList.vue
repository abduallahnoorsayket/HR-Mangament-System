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
                                Project List
                            </div>

                            <router-link :to="{ path: '/add-new-project' }">
                                <a class="float" style="position: absolute; right: 0">
                                    <i class="fas fa-plus-square" style="color: #72e3da; font-size: 33px"></i>
                                </a>
                            </router-link>
                        </div>
                    </div>
                </div>

                <!--Advanced Search Start-->
                <div id="accordion" class="acordinContainer">
                    <div class="card">
                        <div id="headingOne" class="">
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="headerContaier">
                                        <div class="float-left"><h5>Project Search</h5></div>

                                        <div class="float-right">
                                            <i
                                                    id="addIcon"
                                                    class="fa fa-chevron-circle-down"
                                                    style="font: 45px"
                                                    data-toggle="collapse"
                                                    data-target="#collapseOne1"
                                                    aria-expanded="true"
                                                    aria-controls="collapseOne">
                                            </i>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <div data-parent="#accordion" id="collapseOne1" aria-labelledby="headingOne" class="collapse">
                            <div class="card-body">
                                <form action="" @submit.prevent="searchProject">
                                    <div class="form-row">
                                        <!--Project Name-->
                                        <div class="col-md-3">
                                            <label>Project Name </label>
                                            <div class="form-group">
                                                <select
                                                        name="select"
                                                        id="project_name"
                                                        v-model="project_name"
                                                        class="form-control"
                                                        @change="loadBranch();">

                                                    <option value="">select project</option>
                                                    <option v-for="project in projects" :key="project.id"
                                                            :value="project.project_name">
                                                        {{ project.project_name }}
                                                    </option>
                                                </select>
                                            </div>
                                        </div>

                                        <!--Branch-->
                                        <div class="col-md-3">
                                            <label>Branch </label>
                                            <div class="form-group">
                                                <select
                                                        name="select"
                                                        id="branch"
                                                        v-model="branch"
                                                        class="form-control"
                                                        @change="getEmployee();
                                                        getLeaveType();">
                                                    <option value="">select branch</option>
                                                    <option v-for="brnch in all_branches" :key="brnch.id"
                                                            :value="brnch.id">
                                                        {{ brnch.branch_name }}
                                                    </option>
                                                </select>
                                            </div>
                                        </div>

                                        <!--Client-->
                                        <div class="col-md-3">
                                            <label>Client </label>
                                            <select name="select" id="client" v-model="client" class="form-control">
                                                <option value="">select client</option>
                                                <option v-for="client in projects" :key="client.id"
                                                        :value="client.id">
                                                    {{ client.client.client_name }}
                                                </option>
                                            </select>
                                        </div>

                                        <!--Status-->
                                        <div class="col-md-3">
                                            <label>Status</label>
                                            <select
                                                    name="select"
                                                    id="type"
                                                    v-model="status"
                                                    type="choice"
                                                    class="form-control">
                                                <option value="">select client</option>
                                                <option value="PN">Pending</option>
                                                <option value="RN">Running</option>
                                                <option value="CM">Completed</option>
                                                <option value="HO">Handover</option>
                                                <option value="PP">Postponed</option>
                                            </select>

                                        </div>
                                    </div>

                                    <div class="form-row">
                                        <div class="col-md-4"></div>
                                        <div class="col-md-4"></div>

                                        <div class="col-md-4">
                                            <div class="form-group">
                                                <button type="submit" class="btn btn-base float-right"
                                                        @submit.prevent="searchProject">Search
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
                <!--Advanced Search End-->

                <!--Table Data Start-->
                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-striped">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Project Name</th>
                                        <th scope="col">Branch</th>
                                        <th scope="col">Client</th>
                                        <th scope="col">Created Date</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(project, SL) in projects" :key="project.id">
                                        <td>{{ SL + 1 }}</td>
                                        <td>{{ project.project_name }}</td>
                                        <td>{{ project.branch.branch_name }}</td>
                                        <td>{{ project.client.client_name }}</td>
                                        <td>{{ project.created_datetime }}</td>


                                        <td class="td_action">

                                            <router-link class="item"
                                                         :to="{ name: 'ProjectDetails', params: { id: project.id } }">
                                                <i class="fas fa-eye"
                                                   style="color: #00c6ff; padding: 10px"></i>
                                            </router-link>

                                            <router-link class="item"
                                                         :to="{ name: 'UpdateNewProject', params: { id: project.id } }">
                                                <i class="fas fa-edit" style="color: #72e3da"></i>
                                            </router-link>

                                            <i class="fas fa-trash-alt" style="color: firebrick; padding: 10px"
                                               @click="deleteProjectItem(project.id) in projects">
                                            </i>

                                        </td>
                                    </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
                <!--Table Data End-->

                <!--Pagination Start-->
                <div class="row">
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
                </div>
                <!--Pagination End-->
            </div>
        </template>
    </Master>
</template>

<script>
    import Master from "../../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";
    // import Swal from "sweetalert2";

    export default {
        name: "ProjectList",
        components: {Master},
        data() {
            return {
                projects: null,

                project_name: null,
                all_branches: null,
                branch: null,
                client: null,
                status: null,

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
            this.GetProjectList();
            this.loadBranch();
        },

        updated() {
            this.GetProjectList();
        },

        methods: {

            /*Get Project List*/
            GetProjectList: function () {
                var queryParam = {
                    project_name: this.$route.query.project_name,
                    branch: this.$route.query.branch,
                    client: this.$route.query.client,
                    status: this.$route.query.status,

                    page: this.$route.query.page,
                };
                axios.get("projects/", {
                    params: queryParam,
                }).then((response) => {
                    this.projects = response.data.results;

                    this.pagination.count = response.data.count;
                    this.pagination.next = response.data.next;
                    this.pagination.previous = response.data.previous;
                    this.pagination.showing = response.data.results.length;

                    console.log('projects////', this.projects);
                    console.log(response.status);
                });
            },

            /*Delete Project*/
            deleteProjectItem: function (id) {
                Swal.fire({
                    title: "Are you sure?",
                    text: "You won't be able to revert this!",
                    icon: "warning",
                    showCancelButton: true,
                    confirmButtonColor: "#3085d6",
                    cancelButtonColor: "#d33",
                    confirmButtonText: "Yes, delete it!",
                }).then((response) => {
                    if (response.isConfirmed) {
                        axios.delete("projects/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetProjectList();
                            }
                        });
                        Swal.fire(
                            "Deleted!",
                            "Your Project has been deleted!!",
                            "success"
                        );
                    } else {
                        Swal.fire(
                            "Cancelled",
                            "Your Project has not been deleted !",
                            "error"
                        );
                    }
                });
            },

            /*Load Branch*/
            loadBranch: function () {
                const token = localStorage.getItem("token");
                axios
                    .get("branches/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then(
                        function (response) {
                            this.all_branches = response.data;
                        }.bind(this)
                    );
            },

            setPage(page) {
                this.pagination.page = page;
                this.searchProject();
            },

            /*search Project*/
            async searchProject() {
                await this.$router.push({
                    path: "project-list",
                    query: {
                        project_name: this.project_name,
                        branch: this.branch,
                        client: this.client,
                        status: this.status,

                        page: this.pagination.page,
                    },
                });
            },

        },
    };
</script>

<style scoped>
    .table {
        text-align: center;
    }

    .td_action button {
        margin: 3px;
    }
</style>
