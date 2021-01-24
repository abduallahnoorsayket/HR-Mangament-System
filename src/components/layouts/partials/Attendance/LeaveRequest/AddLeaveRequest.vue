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
                            <div>Add Leave Request</div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <form @submit.prevent="submitAddLeave">
                                    <div class="form-group">
                                        <div class="form-row">
                                            <div class="col-md-12">
                                                <div class="position-relative form-group">
                                                    <label>Leave subject </label>
                                                    <input
                                                            placeholder="subject"
                                                            id="department_name"
                                                            v-model="leave_subject"
                                                            type="text"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': leave_req_error && leave_req_error.leave_subject }"
                                                    />

                                                    <!--error handling-->
                                                    <div :class="{ 'invalid-feedback': leave_req_error && leave_req_error.leave_subject }"
                                                         v-if="leave_req_error && leave_req_error.leave_subject">
                                                        {{ leave_req_error.leave_subject[0] }}
                                                    </div>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-12">
                                                <div class="position-relative form-group">
                                                    <label> Leave Description </label>
                                                    <br/>
                                                    <ckeditor :editor="editor" v-model="leave_description"
                                                              :config="editorConfig"></ckeditor>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-4">
                                                <div class="position-relative form-group">
                                                    <label>Employee Name </label>
                                                    <select
                                                            name="select"
                                                            id="employee_name"
                                                            v-model="employee_name"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': leave_req_error && leave_req_error.employee_name }">
                                                        <option value="false" disabled selected>Select User name</option>
                                                        <option v-for="employee in all_employee_name" :key="employee.id" :value="employee.id">
                                                            {{ employee.username }}
                                                        </option>

                                                        <!--error handling-->
                                                        <div :class="{ 'invalid-feedback': leave_req_error && leave_req_error.employee_name }"
                                                             v-if="leave_req_error && leave_req_error.employee_name">
                                                            {{ leave_req_error.employee_name[0] }}
                                                        </div>
                                                    </select>
                                                </div>
                                            </div>

                                            <div class="col-md-4">
                                                <label>Approved by</label>
                                                <select
                                                        name="select"
                                                        id="approved_by"
                                                        v-model="approved_by"
                                                        class="form-control"
                                                        :class="{ 'is-invalid': leave_req_error && leave_req_error.approved_by }">
                                                    <option value="false" disabled selected>Approved by</option>
                                                    <option v-for="employee in all_employee_name" :key="employee.id"
                                                            :value="employee.id">
                                                        {{ employee.username }}
                                                    </option>

                                                    <!--error handling-->
                                                    <div :class="{ 'invalid-feedback': leave_req_error && leave_req_error.approved_by }"
                                                         v-if="leave_req_error && leave_req_error.approved_by">
                                                        {{ leave_req_error.approved_by[0] }}
                                                    </div>
                                                </select>
                                            </div>

                                            <div class="col-md-4">
                                                <label>Select Leave Status </label>
                                                <select
                                                        name="select"
                                                        id="leave-status"
                                                        v-model="select_leave_status"
                                                        type="boolean"
                                                        class="form-control"
                                                        :class="{ 'is-invalid': add_leave_error && add_leave_error.select_leave_status }">
                                                    <option>Approve</option>
                                                    <option>Pending</option>
                                                    <option>Decline</option>
                                                    <span>{{ select_leave_status }}</span>

                                                    <!--error handling-->
                                                    <div :class="{ 'invalid-feedback': add_leave_error && add_leave_error.select_leave_status }"
                                                         v-if="add_leave_error && add_leave_error.select_leave_status">
                                                        {{ add_leave_error.select_leave_status[0] }}
                                                    </div>
                                                </select>
                                            </div>
                                        </div>


                                        <div class="form-row">
                                            <div class="col-md-4">
                                                <div class="position-relative form-group">
                                                    <label>Start Date </label>
                                                    <input
                                                            name="Start-date"
                                                            id="Start-date"
                                                            v-model="application_start_date"
                                                            placeholder=""
                                                            type="date"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': leave_req_error && leave_req_error.application_start_date }"
                                                    />

                                                    <!--error handling-->
                                                    <div :class="{ 'invalid-feedback': leave_req_error && leave_req_error.application_start_date }"
                                                         v-if="leave_req_error && leave_req_error.application_start_date">
                                                        {{ leave_req_error.application_start_date[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <div class="col-md-4">
                                                <label>End Date</label>
                                                <input
                                                        name="End-date"
                                                        id="End-date"
                                                        v-model="application_end_date"
                                                        placeholder=""
                                                        type="date"
                                                        class="form-control"
                                                        :class="{ 'is-invalid': leave_req_error && leave_req_error.application_end_date }"/>

                                                <!--error handling-->
                                                <div
                                                        :class="{ 'invalid-feedback': leave_req_error && leave_req_error.application_end_date }"
                                                        v-if="leave_req_error && leave_req_error.application_end_date">
                                                    {{ leave_req_error.application_end_date[0] }}
                                                </div>
                                            </div>

                                            <div class="col-md-4">
                                                <label>Leave Type </label>
                                                <select
                                                        name="select"
                                                        id="leave_type"
                                                        v-model="leave_type"
                                                        class="form-control"
                                                        :class="{ 'is-invalid': leave_req_error && leave_req_error.leave_type }">
                                                    <option value="false" disabled selected>Select Leave Type
                                                    </option>
                                                    <option v-for="leavetype in all_leave_types" :key="leavetype.id"
                                                            :value="leavetype.id">
                                                        {{ leavetype.leave_type }}
                                                    </option>

                                                    <!--error handling-->
                                                    <div :class="{ 'invalid-feedback': leave_req_error && leave_req_error.leave_type }"
                                                         v-if="leave_req_error && leave_req_error.leave_type">
                                                        {{ leave_req_error.leave_type[0] }}
                                                    </div>
                                                </select>
                                            </div>
                                        </div>

                                        <div class="form-group">
                                            <button @submit.prevent="submitAddLeave"
                                                    class="mt-2 btn btn-success btn-lg float-right">Add Leave
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
    import Swal from "sweetalert2";
    import ClassicEditor from '@ckeditor/ckeditor5-build-classic';
    import CKEditor from '@ckeditor/ckeditor5-vue';

    export default {
        name: "AddLeaveRequest",
        components: {
            Master,
            ckeditor: CKEditor.component
        },
        data() {
            return {
                editor: ClassicEditor,

                leave_subject: null,
                leave_description: null,
                application_start_date: null,
                application_end_date: null,
                leave_type: null,
                username: null,
                select_leave_status: null,
                approved_by: null,
                employee_name: null,

                all_leave_types: null,
                all_employee_name: null,

                leave_req_error: {
                    leave_subject: null,
                    leave_description: null,
                    application_start_date: null,
                    application_end_date: null,
                    leave_type: null,
                    username: null,
                    select_leave_status: null,
                    approved_by: null,
                    employee_name: null,
                },
            };
        },

        methods: {
            loadLeaveTypes() {
                const token = localStorage.getItem("token");
                axios
                    .get("leave-types-list/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_leave_types = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            loadEmployeeName() {
                const token = localStorage.getItem("token");
                axios
                    .get("users/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_employee_name = response.data.results;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            async submitAddLeave() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("leaves/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        leave_subject: this.leave_subject,
                        leave_description: this.leave_description,
                        application_start_date: this.application_start_date,
                        application_end_date: this.application_end_date,
                        leave_type: this.leave_type,
                        employee_name: this.employee_name,
                        select_leave_status: this.select_leave_status,
                        approved_by: this.approved_by,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully Added  Leave requset..",
                        }).then((result) => {
                            this.$router.push("all-leaves");
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.leave_req_error = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
            },
        },

        created() {
            this.loadLeaveTypes();
            this.loadEmployeeName();
        },
    };
</script>

<style scoped></style>
