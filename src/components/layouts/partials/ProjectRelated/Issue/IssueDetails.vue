<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon" v-if="issues">
                                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
                            </div>

                            Issue Details

                            <template v-if="issues">

                                <router-link class="nav-link" :to="{name: 'IssueList'}">
                                    <button class="mt-2 btn btn-secondary" style="position: absolute; right: 175px">Back
                                    </button>
                                </router-link>


                                <router-link class="nav-link" v-if="issues"
                                             :to="{ name: 'UpdateIssue', params: { id: issues.id } }">
                                    <button class="mt-2 btn btn-info" style="position: absolute; right: 118px">Edit
                                    </button>
                                </router-link>


                                <!--<button class="mt-2 btn btn-info" style="position: absolute; right: 118px">Edit</button>-->

                                <router-link class="nav-link" :to="{name: 'AddSubIssue', params: { id: issues.id }}">
                                    <button class="mt-2 btn btn-success" style="position: absolute; right: 0">Add Sub
                                        Issue
                                    </button>
                                </router-link>

                            </template>

                        </div>
                    </div>
                </div>

                <div class="row" v-if="issues">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-bordered">
                                    <tbody>
                                    <!--1st row = Project Name & Subject-->
                                    <tr>
                                        <th>Project Name</th>
                                        <td>{{issues.project.project_name}}</td>

                                        <th>Subject</th>
                                        <td>{{issues.subject}}</td>

                                    </tr>
                                    <!--2nd row = Start Date & Due Date-->
                                    <tr>
                                        <th>Start Date</th>
                                        <td>{{issues.start_date}}</td>

                                        <th>Due Date</th>
                                        <td>{{issues.due_date}}</td>
                                    </tr>
                                    <!--3rd row = Priority & Estimated Time-->
                                    <tr>
                                        <th>Priority</th>
                                        <td>{{issues.work_priority.name}}</td>

                                        <th>Estimated Time</th>
                                        <td>{{issues.estimated_time + ' Hours'}}</td>
                                    </tr>
                                    <!--4rth row = Status & Work Type-->
                                    <tr>
                                        <th>Status</th>
                                        <td>{{issues.work_status.name}}</td>

                                        <th>Work Type</th>
                                        <td>{{issues.work_type.name}}</td>
                                    </tr>
                                    <!--5th row = Privacy Status & Assignee-->
                                    <tr>
                                        <th>Privacy Status</th>
                                        <td>{{issues.privacy_status}}</td>

                                        <template v-for="issues in issues.assignee" :key="issues.id">
                                            <th>Assignee</th>
                                            <td>{{ issues.first_name + ' ' + issues.last_name }}</td>
                                        </template>

                                    </tr>
                                    </tbody>
                                </table>
                            </div>

                            <!--Issue Description & Project Files-->
                            <div class="col-md-12 mb-3">
                                <strong>Issues Description:</strong> {{issues.description}}
                                <br>
                                <br>

                                <strong>Project Files:</strong>
                                <template v-for="(issues, index) in issues.files" :key="index">
                                    <a :href="issues.upload_files"
                                       class="btn btn-info btn-sm ml-1 mb-3">{{issues.file_name}}</a>
                                </template>
                            </div>
                        </div>
                    </div>
                </div>


                <!--Sub Issue Showing Area-->
                <h3 class="mt-5">Working History</h3>
                <template class="row" v-if="issues">
                    <div class="col-md-12 card" v-for="issue in issues.issue_sub_issue" :key="issue">
                        <!--Edit Button-->
                        <router-link class="item" style="position: absolute; right: 0"
                                     :to="{ name: 'UpdateSubIssue', params: { id: issue.id } }">
                            <button class="btn-secondary btn-sm"><i class="fas fa-edit"></i> Edit</button>
                        </router-link>

                        <h5>{{issue.subject}}</h5>
                        <p class="sub_issue_data" v-for="issue in issues.assignee" :key="issue">by
                            <strong>{{issue.first_name + ' ' + issue.last_name}}</strong>
                        </p>
                        <p class="sub_issue_data">Sub issue progress <strong>{{issue.progress + '%'}}</strong></p>
                        <p class="sub_issue_data">Updated at <strong>{{issue.updated_at}}</strong></p>

                        <p class="sub_issue_data mt-3"><strong>Work Description: </strong>
                            <span v-html="issue.description"></span> <!--v-html = reduce <p> tag in description-->
                        </p>
                    </div>
                </template>
            </div>
        </template>
    </Master>
</template>

<script>
    import Master from "../../../Master";
    import axios from "axios";
    // import Swal from "sweetalert2";

    export default {
        name: "IssueDetails",
        components: {Master},
        data() {
            return {
                issues: null,

                sub_issue_list: null
            };
        },

        methods: {
            GetIssueDetails() {
                axios.get(`issues/${this.$route.params.id}/`).then(
                    (response) => {
                        this.issues = response.data;
                        console.log(this.issues);
                        console.log(response.status);
                    });
            },
        },

        created() {
            this.GetIssueDetails()
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

    .sub_issue_data {
        font-size: 16px;
    }

    .edit_btn {
        background-color: rgba(87, 82, 82, 0.52);
        border: none;
        color: white;
        padding: 12px 16px;
        font-size: 16px;
        cursor: pointer;
    }
</style>