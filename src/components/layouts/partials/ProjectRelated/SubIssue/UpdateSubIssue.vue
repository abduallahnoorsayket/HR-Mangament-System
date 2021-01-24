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
                <!--<nav class="navbar navbar-expand-lg navbar navbar navbar-light" style="background-color: #e3f2fd;">
                    <div class="collapse navbar-collapse" id="navbarNav">
                        <ul class="navbar-nav">
                            <li class="nav-item ">
                                <router-link class="nav-link" :to="{name: 'ProjectDetails'}">
                                    Project Overview
                                </router-link>
                            </li>
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'IssueList'}">
                                    Issues
                                </router-link>
                            </li>
                            <li class="nav-item active">
                                <router-link class="nav-link" :to="{name: 'AddIssue'}">
                                    <strong>Add New Issue</strong>
                                </router-link>
                            </li>
                            <li class="nav-item">
                                <a class="nav-link " href="#">Activity</a>
                            </li>
                            <li class="nav-item">
                                <a class="nav-link" href="#">Gannt Chart </a>
                            </li>
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'FilesList'}">
                                    Files
                                </router-link>
                            </li>
                        </ul>
                    </div>
                </nav>-->

                <!--Sub Issue Form-->
                <div class="row">
                    <div class="col-12">

                        <div class="main-card card">
                            <div class="card-body">
                                <h5> Update Sub Issue</h5>
                                <hr>
                                <form @submit.prevent="updateSubIssueForm">
                                    <div class="form-group">

                                        <!--Work Type & Subject-->
                                        <div class="form-row">
                                            <!--Work Type-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Work Type *</label>
                                                    <select name="select"
                                                            v-model="work_type"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': error_add_holiday && error_add_holiday.type }">
                                                        <option value="false" disabled selected>Select Work Type
                                                        </option>
                                                        <option v-for="work_type in all_work_type" :key="work_type.id"
                                                                :value="work_type.id">
                                                            {{work_type.name}}
                                                        </option>
                                                    </select>

                                                    <div :class="{ 'invalid-feedback': error_add_holiday && error_add_holiday.type }"
                                                         v-if="error_add_holiday && error_add_holiday.type">
                                                        {{ error_add_holiday.type[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <!--Subject-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Subject * </label>
                                                    <input placeholder="Subject" id="project_name"
                                                           v-model="subject" type="text" class="form-control"
                                                           :class="{ 'is-invalid': leave_req_error && leave_req_error.leave_subject }"/>

                                                    <!--error handling-->
                                                    <div :class="{ 'invalid-feedback': leave_req_error && leave_req_error.leave_subject }"
                                                         v-if="leave_req_error && leave_req_error.leave_subject">
                                                        {{ leave_req_error.leave_subject[0] }}
                                                    </div>
                                                </div>
                                            </div>
                                        </div>

                                        <!--Priority & Start Date-->
                                        <div class="form-row">
                                            <!--Priority-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Priority</label>
                                                    <select name="select" v-model="work_priority" type="choice"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': error_add_holiday && error_add_holiday.type }">
                                                        <option disabled selected>Select Work Type</option>
                                                        <option v-for="work_priority in all_work_priority"
                                                                :key="work_priority.id"
                                                                :value="work_priority.id">{{ work_priority.name }}
                                                        </option>
                                                    </select>
                                                    <div :class="{ 'invalid-feedback': error_add_holiday && error_add_holiday.type }"
                                                         v-if="error_add_holiday && error_add_holiday.type">
                                                        {{ error_add_holiday.type[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <!--Start Date-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Start Date </label>
                                                    <input
                                                            name="updated_at[]"
                                                            type="date"
                                                            v-model="updated_at"
                                                            class="form-control">
                                                </div>
                                            </div>
                                        </div>

                                        <!--Working Hour & Range-->
                                        <div class="form-row">

                                            <!--Working Hour-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Working Hour </label>
                                                    <input placeholder="Hours"
                                                           v-model="working_hour"
                                                           class="form-control"
                                                    />
                                                </div>
                                            </div>

                                            <!--Range-->
                                            <div class="col-md-6">
                                                <label>Range</label>
                                                <input
                                                        class="form-control"
                                                        type="range"
                                                        v-model="progress"
                                                        min="1"
                                                        max="100"
                                                        oninput="this.nextElementSibling.value = this.value"
                                                />
                                                <output class="float-right">{{progress}}</output>
                                            </div>
                                        </div>

                                        <!--Description-->
                                        <div class="form-row">
                                            <div class="col-md-12">
                                                <div class="position-relative form-group">
                                                    <label> Description *</label>
                                                    <br/>
                                                    <ckeditor :editor="editor" v-model="description"
                                                              :config="editorConfig"></ckeditor>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-group mt-3">
                                            <button @submit.prevent="updateSubIssueForm"
                                                    class="mt-2 btn btn-success btn-lg float-right">Create Sub Issue
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
    import axios from "axios";
    // import Swal from "sweetalert2";
    import Master from "../../../Master";
    import ClassicEditor from "@ckeditor/ckeditor5-build-classic";
    import CKEditor from "@ckeditor/ckeditor5-vue";
    import Swal from "sweetalert2";
    // import Swal from "sweetalert2";

    export default {
        name: "UpdateSubIssue",
        components: {Master, ckeditor: CKEditor.component},
        data() {
            return {

                subject: null,
                description: null,
                issue: null,
                work_type: null,
                work_priority: null,
                updated_at: null,
                working_hour: null,
                progress: null,

                // for CKEditor
                editor: ClassicEditor,

                load_issue: null,

                all_work_type: null,
                all_work_priority: null,

            };
        },
        created() {
            this.loadWorkType();
            this.loadWorkPriority();
            this.getIssueList();
            this.getSubIssueData();
        },
        methods: {

            // getSubIssueData
            getSubIssueData: function () {
                axios.get(`sub-issues/${this.$route.params.id}/`).then(
                    (response) => {

                        this.subject = response.data.subject;
                        this.description = response.data.description;
                        this.work_type = response.data.work_type.id;
                        this.work_priority = response.data.work_priority.id;
                        this.updated_at = response.data.updated_at;
                        this.working_hour = response.data.working_hour;
                        this.progress = response.data.progress;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
            },

            // updateSubIssue
            updateSubIssueForm() {
                const token = localStorage.getItem("token");
                axios
                    .put("sub-issues/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        subject: this.subject,
                        description: this.description,
                        work_type: this.work_type,
                        work_priority: this.work_priority,
                        updated_at: this.updated_at,
                        working_hour: this.working_hour,
                        progress: this.progress,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit a Sub Issue..",
                        }).then((response) => {
                            this.$router.push({name: "IssueDetails"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.department_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
            },


            // Submit Sub Issue Form
            async submitAddSubIssueForm() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("sub-issues/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },

                        subject: this.subject,
                        description: this.description,
                        issue: this.load_issue,
                        work_type: this.work_type,
                        work_priority: this.work_priority,
                        updated_at: this.updated_at,
                        working_hour: this.working_hour,
                        progress: this.progress,

                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully create a Sub Issue..",
                        }).then((result) => {
                            this.$router.push({name: "IssueDetails"});
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.group_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
            },


            
            // Load Work Type into dropdown
            loadWorkType() {
                const token = localStorage.getItem("token");
                axios
                    .get("work-types/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    }).then((response) => {
                    this.all_work_type = response.data;
                })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            // Load Work Priority into dropdown
            loadWorkPriority() {
                const token = localStorage.getItem("token");
                axios
                    .get("work-priorities/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    }).then((response) => {
                    this.all_work_priority = response.data;
                })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            // getIssueList
            getIssueList() {
                axios.get(`issues/${this.$route.params.id}/`).then(
                    (response) => {
                        this.load_issue = response.data.id;
                        console.log("----UpdateIssue---", this.load_issue);
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

</style>