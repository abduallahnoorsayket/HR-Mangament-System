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
                            <div>Update Issue</div>
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
                </nav>

                <!--Issue Form-->
                <div class="row">
                    <div class="col-12">

                        <div class="main-card card">
                            <div class="card-body">
                                <h5> Add New Issue</h5>
                                <hr>
                                <form @submit.prevent="submitAddIssueForm">
                                    <div class="form-group">

                                        <!--Work Type & privacy status-->
                                        <div class="form-row">
                                            <!--Work Type-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Work Type *</label>
                                                    <select name="select"
                                                            id="work_type"
                                                            v-model="work_type"
                                                            class="form-control">

                                                        <option value="false" disabled selected>Select Work Type
                                                        </option>
                                                        <option v-for="work_type in all_work_type" :key="work_type.id"
                                                                :value="work_type.id">
                                                            {{work_type.name}}
                                                        </option>
                                                    </select>
                                                </div>
                                            </div>

                                            <!--privacy status-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Privacy Status *</label>
                                                    <select name="select" v-model="privacy_status" type="choice"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': error_add_holiday && error_add_holiday.type }">
                                                        <option disabled selected>Select Privacy Status</option>
                                                        <option value="Pub">Public</option>
                                                        <option value="Pri">Private</option>
                                                        <option value="Oth">Others</option>

                                                    </select>
                                                    <div :class="{ 'invalid-feedback': error_add_holiday && error_add_holiday.type }"
                                                         v-if="error_add_holiday && error_add_holiday.type">
                                                        {{ error_add_holiday.type[0] }}
                                                    </div>
                                                </div>
                                            </div>
                                        </div>

                                        <!--Issue subject-->
                                        <div class="form-row">
                                            <div class="col-md-12">
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

                                        <!--Status & Parent Issue-->
                                        <div class="form-row">
                                            <!--Status-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Status *</label>
                                                    <select name="select" v-model="work_status" type="choice"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': error_add_holiday && error_add_holiday.type }">
                                                        <option value="false" disabled selected>Select Issue Type
                                                        </option>
                                                        <option v-for="work_statuses in all_work_statuses"
                                                                :key="work_statuses.id"
                                                                :value="work_statuses.id">{{ work_statuses.name }}
                                                        </option>
                                                    </select>
                                                    <div :class="{ 'invalid-feedback': error_add_holiday && error_add_holiday.type }"
                                                         v-if="error_add_holiday && error_add_holiday.type">
                                                        {{ error_add_holiday.type[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <!--Parent Issue-->
                                            <!--<div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Parent Issue</label>
                                                    <select name="select" v-model="parent_issue" type="choice"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': error_add_holiday && error_add_holiday.type }">
                                                        <option value="PN">Select Issue Type</option>
                                                        <option value="PN">Database Migration</option>
                                                        <option value="RN">Upload Image</option>
                                                        <option value="CM">Development Model And Migrations</option>
                                                    </select>
                                                    <div :class="{ 'invalid-feedback': error_add_holiday && error_add_holiday.type }"
                                                         v-if="error_add_holiday && error_add_holiday.type">
                                                        {{ error_add_holiday.type[0] }}
                                                    </div>
                                                </div>
                                            </div>-->

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
                                        </div>

                                        <!--Priority & Start Date-->
                                        <div class="form-row">
                                            <!--Start Date-->
                                            <div class="col-md-6">
                                                <label>Start Date</label>
                                                <input
                                                        name="joining-date"
                                                        id="date"
                                                        v-model="start_date"
                                                        type="date"
                                                        class="form-control"
                                                />
                                            </div>

                                            <!--Due Date-->
                                            <div class="col-md-6">
                                                <label>Due Date</label>
                                                <input
                                                        name="joining-date"
                                                        id="due_date"
                                                        v-model="due_date"
                                                        type="date"
                                                        class="form-control"
                                                />
                                            </div>
                                        </div>

                                        <!--Upload File & Assignee-->
                                        <div class="form-row">
                                            <!--Upload File-->
                                            <div class="col-md-12">
                                                <div class="position-relative form-group ">
                                                    <template v-for="(file, k) in files_related" :key="k">
                                                        <div class="row mt-4">
                                                            <div class="col-md-6">
                                                                <label> Upload Document :</label>
                                                                <input type="file" class="form-control"
                                                                       @change="handleImage($event,k)" id="file"/>
                                                            </div>
                                                            <div class="col-md-6">
                                                                <label>Document Name</label>
                                                                <input class="form-control" type="text"
                                                                       autocomplete="off" v-model="file.file_name">
                                                            </div>
                                                            <div class="col-md-2" v-if="k==0">
                                                            </div>
                                                            <div class="col-md-2" v-else>
                                                                <button class="btn btn-sm btn-del remove mt-1"
                                                                        style="color: firebrick; font-size: 20px; "><i
                                                                        class="fas fa-trash-alt "
                                                                        @click="deleteRow(k)"></i></button>
                                                            </div>
                                                        </div>
                                                        <div v-if="error_data && error_data.non_field_errors"
                                                             class="col-md-6 alert alert-danger alert-dismissible fade show text-center "
                                                             role="alert">
                                                            <!-- {{ error_data.files[0] }} -->
                                                            These fields may not be blank
                                                            <button type="button" class="close" data-dismiss="alert"
                                                                    aria-label="Close">
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

                                        <!--Estimated time & Due Date-->
                                        <div class="form-row mt-3">
                                            <!--Estimated time-->
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Estimated time </label>
                                                    <input placeholder="Hours"
                                                           v-model="estimated_time"
                                                           type="text"
                                                           class="form-control"
                                                    />
                                                </div>
                                            </div>

                                            <!--Assignee-->
                                            <div class="col-md-6 permissions">
                                                <label>Assignee</label><br>
                                                <input type="text" v-model="searchQuery" placeholder="filter"
                                                       style="outline: none;">
                                                <div class="grp-per" v-if="all_users">

                                                    <div class="position-relative form-check"
                                                         v-for="user in resultQuery" :key="user.id">
                                                        <label class="form-check-label">
                                                            <input
                                                                    type="checkbox"
                                                                    :id="user"
                                                                    :value="user.id"
                                                                    v-model="assignee"
                                                                    class="form-check-input"/>
                                                            {{ user.first_name + ' ' + user.last_name}}
                                                        </label>

                                                    </div>
                                                </div>
                                            </div>
                                        </div>


                                        <!--Optional Description & Due Date-->
                                        <!--<div class="form-row">
                                            &lt;!&ndash;Optional Description&ndash;&gt;
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label>Description</label>
                                                    <input
                                                            placeholder="Optional Description"
                                                            id="optional_description"
                                                            v-model="project_name"
                                                            type="text"
                                                            class="form-control"
                                                    />
                                                </div>
                                            </div>

                                            &lt;!&ndash;Range&ndash;&gt;
                                            <div class="col-md-6">
                                                <label>Range</label>
                                                <input
                                                        class="form-control"
                                                        type="range"
                                                        value="24"
                                                        min="1"
                                                        max="100"
                                                        oninput="this.nextElementSibling.value = this.value"
                                                />
                                                <output class="float-right">24</output>
                                            </div>
                                        </div>-->


                                        <div class="form-group mt-3">
                                            <button @submit.prevent="submitAddIssueForm"
                                                    class="mt-2 btn btn-success btn-lg float-right">Create Issue
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
    import Swal from "sweetalert2";
    import Master from "../../../Master";
    import ClassicEditor from "@ckeditor/ckeditor5-build-classic";
    import CKEditor from "@ckeditor/ckeditor5-vue";


    export default {
        name: "UpdateIssue",
        components: {Master, ckeditor: CKEditor.component},
        data() {
            return {
                work_type: null,
                privacy_status: null,
                subject: null,
                description: null,
                work_status: null,
                parent_issue: null,
                work_priority: null,
                start_date: null,
                assignee: [],
                due_date: null,
                upload_file: null,
                estimated_time: null,

                files_related: [],

                all_work_type: null,
                all_work_statuses: null,
                all_work_priority: null,
                all_users: null,
                project_details: null,
                searchQuery: null,


                // for CKEditor
                editor: ClassicEditor,
            };
        },

        methods: {

            // Submit Add Issue Form
            async submitAddIssueForm() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("issues/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },

                        work_type: this.work_type,
                        privacy_status: this.privacy_status,
                        subject: this.subject,
                        description: this.description,
                        work_status: this.work_status,
                        parent_issue: this.parent_issue,
                        work_priority: this.work_priority,
                        start_date: this.start_date,
                        assignee: this.assignee,
                        due_date: this.due_date,
                        upload_file: this.upload_file,
                        estimated_time: this.estimated_time,

                        files: this.files_related,
                        project: this.project_details
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully create a Issue..",
                        }).then((result) => {
                            this.$router.push({name: "IssueList"});
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


            /*Get Issue Data*/
            getIssueData: function () {
                axios.get(`issues/${this.$route.params.id}/`).then(
                    (response) => {
                        this.work_type = response.data.work_type.id;
                        this.privacy_status = response.data.privacy_status.id;
                        this.subject = response.data.subject;
                        this.description = response.data.description;
                        this.work_status = response.data.work_status.id;
                        this.work_priority = response.data.work_priority.id;
                        this.start_date = response.data.start_date;
                        this.assignee = response.data.assignee.map((assignee) => assignee.id);
                        this.due_date = response.data.due_date;

                        this.estimated_time = response.data.estimated_time;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
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

            // Load Work Status into dropdown
            loadWorkStatus() {
                const token = localStorage.getItem("token");
                axios
                    .get("work-statuses/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    }).then((response) => {
                    this.all_work_statuses = response.data;
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

            // Load User Assignee
            loadUsers() {
                const token = localStorage.getItem("token");
                axios
                    .get("users/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    }).then((response) => {
                    this.all_users = response.data.results;
                })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            // getProjectDetails
            getProjectDetails() {
                axios.get(`projects/${this.$route.params.id}/`).then(
                    (response) => {
                        this.project_details = response.data.id;
                        console.log("----project_details---", this.project_details);
                        // this.form_data.leave_type = this.form_data.leave_type.id;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
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
                                this.files_related.push({
                                    file_name: "",
                                    upload_files: "",
                                    content_type: this.content_id,
                                });
                            }
                        });
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
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
                if (index == 0) {
                    alert("Can not be deleted !!");
                } else if (index > 0) {
                    this.files_related.splice(index, 1);
                }
            },

            /*for file upload*/
            handleImage(e, index) {
                let files = e.target.files || e.dataTransfer.files;
                if (!files.length) return;
                this.getBase64(files[0], index);
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
        },

        created() {
            this.getIssueData();
            this.loadWorkType();
            this.loadWorkStatus();
            this.loadWorkPriority();
            this.loadUsers();
            this.getProjectDetails();
            this.loadContentType();
        },

        computed: {
            resultQuery() {
                if (this.searchQuery) {
                    return this.all_users.filter((item) => {
                        return this.searchQuery.toLowerCase().split(' ').every(v => item.first_name.toLowerCase().includes(v))
                    })
                } else {
                    return this.all_users;
                }
            }
        }
    };
</script>

<style scoped>
    .nav-item {
        margin: 0px 30px;
    }
</style>