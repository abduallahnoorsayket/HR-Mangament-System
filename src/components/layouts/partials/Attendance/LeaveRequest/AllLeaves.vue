<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-procedures icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>All Leaves</div>
                            <router-link :to="{ path: '/add-leaves-request' }">
                                <a class="float" style="position: absolute; right: 0">
                                    <button type="button" class="btn btn-success" style="font-size: 15px">
                                        <span class="fas fa-plus-square"></span> Add Leave Request
                                    </button>
                                </a>
                            </router-link>
                        </div>
                    </div>
                </div>
                <!-- accordion search -->
                <div id="accordion" class="acordinContainer">
                    <div class="card">
                        <div id="headingOne" class="">
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="headerContaier">
                                        <div class="float-left"><h5>Advanced Search</h5></div>

                                        <div class="float-right">
                                            <i
                                                    id="addIcon"
                                                    class="fa fa-chevron-circle-down"
                                                    style="font: 45px"
                                                    data-toggle="collapse"
                                                    data-target="#collapseOne1"
                                                    aria-expanded="true"
                                                    aria-controls="collapseOne"
                                            ></i>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <div data-parent="#accordion" id="collapseOne1" aria-labelledby="headingOne" class="collapse">
                            <div class="card-body">
                                <form action="" @submit.prevent="searchLeave">
                                    <div class="form-row">
                                        <div class="col-md-4">
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

                                        <div class="col-md-4">
                                            <label>Employee Name </label>
                                            <div class="form-group">
                                                <select name="select" id="employee_name" v-model="employee_name"
                                                        class="form-control">
                                                    <!-- <option value="" disabled selected>
                                                    Select department
                                                    </option> -->
                                                    <option value="">select employee</option>
                                                    <option v-for="employee in all_employees" :key="employee.id"
                                                            :value="employee.id">
                                                        {{ employee.first_name }}
                                                    </option>
                                                </select>
                                            </div>
                                        </div>

                                        <div class="col-md-4">
                                            <label>Application start date</label>
                                            <input name="address" id="sdate" v-model="application_start_date"
                                                   placeholder=" start date" type="date" class="form-control"/>
                                        </div>
                                        <div class="col-md-4">
                                            <label>Leave subject </label>
                                            <div class="form-group">
                                                <input class="form-control" type="text" name="" id=""
                                                       v-model="leave_subject"/>
                                            </div>
                                        </div>
                                        <div class="col-md-4">
                                            <label for="exampleEmail11" class="">Leave Type </label>
                                            <select name="select" id="type" v-model="leave_type" class="form-control">
                                                <option value="">select leave type</option>
                                                <option v-for="leavetype in all_leaveTypes" :key="leavetype.id"
                                                        :value="leavetype.id">
                                                    {{ leavetype.leave_type }}
                                                </option>
                                            </select>
                                        </div>

                                        <div class="col-md-4">
                                            <label for="exampleEmail11" class="">Select Leave Status</label>

                                            <select name="select" id="type" v-model="select_leave_status" type="choice"
                                                    class="form-control">
                                                <option value="">select leave status</option>
                                                <option value="Pending">Pending</option>
                                                <option value="Approve">Approved</option>
                                                <option value="Decline">Decline</option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="form-row">
                                        <div class="col-md-4"></div>
                                        <div class="col-md-4"></div>

                                        <div class="col-md-4">
                                            <label></label>
                                            <div class="form-group">
                                                <button type="submit" class="btn btn-base float-right">Search <i
                                                        class="fa fa-search"></i>&nbsp;
                                                </button>
                                            </div>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <!--All Leave Types-->
                <div class="row mt-4">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <div class="mb-4"></div>

                                <!--Data Table-->
                                <table class="table table-striped">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Leave Subject</th>
                                        <th scope="col">Employee Name</th>
                                        <th scope="col">Date</th>
                                        <th scope="col">Total Days</th>
                                        <th scope="col">Leave Types</th>
                                        <th scope="col">Status</th>
                                        <th scope="col">Branch</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(leave, SL) in all_leaves" :key="leave.id">
                                        <td>{{ SL + 1 }}</td>
                                        <td>{{ leave.leave_subject }}</td>

                                        <td>
                                            <span v-if="leave.employee_name">
                                            {{ leave.employee_name.first_name + " " + leave.employee_name.last_name }}
                                            </span>
                                        </td>


                                        <!--<router-link :to="{name: 'UserDetails',params: { id: leave.id+1 }}">
                                            <td>
                                                <span v-if="leave.employee_name" style="text-decoration: none;">
                                                {{ leave.employee_name.first_name + " " + leave.employee_name.last_name }}
                                                </span>
                                            </td>
                                        </router-link>-->


                                        <td>{{ leave.application_start_date + " to " + leave.application_end_date }}
                                        </td>
                                        <td>{{ leave.apply_day_count }}</td>

                                        <td>
                                            <span v-if="leave.leave_type">
                                                {{ leave.leave_type.leave_type }}
                                            </span>
                                        </td>

                                        <td>{{ leave.select_leave_status }}</td>

                                        <td>
                                            <span v-if="leave.leave_type.branch">
                                                {{ leave.leave_type.branch.branch_name }}
                                            </span>
                                        </td>

                                        <td class="td_action">
                                            <router-link class="item"
                                                         :to="{ name: 'AllLeavesDetails', params: { id: leave.id } }">
                                                <i class="fas fa-eye" style="color: #72e3da; padding: 10px"></i>
                                            </router-link>

                                            <router-link class="item"
                                                         :to="{ name: 'UpdateLeaveReq', params: { id: leave.id } }">
                                                <i class="fas fa-edit" style="color: #72e3da"></i>
                                            </router-link>

                                            <i class="fas fa-trash-alt" style="color: #b71c1c; padding: 10px"
                                               @click="deleteAllLeavesItem(leave.id) in all_leaves"> </i>
                                        </td>
                                    </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                    </div>
                </div>
                <br/>
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
            </div>
        </template>
    </Master>
</template>

<script>
    import Master from "../../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "AllLeaves",
        components: {Master},
        data() {
            return {
                users: null,
                all_leaves: null,
                leave_subject: null,
                employee_name: null,
                leave_type: null,
                all_leaveTypes: null,
                select_leave_status: null,
                application_start_date: null,
                application_end_date: null,
                all_departments: null,
                all_branches: null,
                branch: null,
                department: null,
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
// this.branch = null;
// this.leave_subject = null;
// this.employee_name = null;
// this.leave_type = null;
// this.select_leave_status = null;
// this.application_start_date = null;
// this.application_end_date = null;
            console.log("updated");
        },

        methods: {
            initialLoad: function () {
                this.GetAllLeavesList();
                this.loadBranch();
            },

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
            getEmployee: function () {
                axios
                    .get("users/", {
                        params: {
                            branch: this.branch,
                        },
                    })
                    .then(
                        function (response) {
                            this.all_employees = response.data.results;
                        }.bind(this)
                    );
            },

            getLeaveType: function () {
                axios
                    .get("leave-types/", {
                        params: {
                            branch: this.branch,
                        },
                    })
                    .then(
                        function (response) {
                            this.all_leaveTypes = response.data.results;
                        }.bind(this)
                    );
            },

            GetAllLeavesList: function () {
                var queryParam = {
                    branch: this.$route.query.branch,
                    leave_subject: this.$route.query.leave_subject,
                    employee_name: this.$route.query.employee_name,
                    application_start_date: this.$route.query.application_start_date,
                    application_end_date: this.$route.query.application_end_date,
                    leave_type: this.$route.query.leave_type,
                    select_leave_status: this.$route.query.select_leave_status,
                    page: this.$route.query.page,
                };

                axios
                    .get("leaves/", {
                        params: queryParam,
                    })
                    .then((response) => {
                        this.all_leaves = response.data.results;
                        this.pagination.count = response.data.count;
                        this.pagination.next = response.data.next;
                        this.pagination.previous = response.data.previous;
                        this.pagination.showing = response.data.results.length;
                        console.log(this.all_leaves);
                        console.log(response.status);
                    });
            },

            deleteAllLeavesItem: function (id) {
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
                        axios.delete("leaves/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetAllLeavesList();
                            }
                        });
                        Swal.fire("Deleted!", "leave has been deleted!!", "success");
                    } else {
                        Swal.fire("Cancelled", "leave has not been deleted !", "error");
                    }
                });
            },

            setPage(page) {
                this.pagination.page = page;
                this.searchLeave();
            },
            async searchLeave() {
                console.log(this.searchSubject);

                await this.$router.push({
                    path: "all-leaves",
                    query: {
                        leave_subject: this.leave_subject,
                        employee_name: this.employee_name,
                        application_start_date: this.application_start_date,
                        application_end_date: this.application_end_date,
                        leave_type: this.leave_type,
                        select_leave_status: this.select_leave_status,
                        branch: this.branch,
                        page: this.pagination.page,
                    },
                });
            },
        },
    };
</script>

<style scoped>
    .inline {
        display: inline-block;
    }

    .btn-base {
        color: #fff;
        background-color: #3ac47d;
        border-color: #3ac47d;
        margin-top: 2px;
    }

    .headerContaier {
        margin: 0px 20px 20px 20px;
    }

    .card {
        margin-top: 20px;
        padding-top: 20px;
        margin-bottom: 20px;
    }
</style>