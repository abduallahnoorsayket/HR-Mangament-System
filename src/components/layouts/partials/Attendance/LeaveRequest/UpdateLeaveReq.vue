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
                            <div>Edit Leave Request</div>
                        </div>
                    </div>
                </div>

                <div class="row mb-4">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <form @submit.prevent="submitEditLeave" v-if="form_data" autocomplete="off">
                                    <div class="form-group">
                                        <div class="form-row">
                                            <div class="col-md-12">
                                                <div class="position-relative form-group">
                                                    <label>Leave subject </label>
                                                    <input
                                                            placeholder="subject"
                                                            id="leave_subject"
                                                            v-model="form_data.leave_subject"
                                                            type="text"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': update_leave_error && update_leave_error.leave_subject }"
                                                    />
                                                    <div
                                                            :class="{ 'invalid-feedback': update_leave_error && update_leave_error.leave_subject }"
                                                            v-if="update_leave_error && update_leave_error.leave_subject"
                                                    >
                                                        {{ update_leave_error.leave_subject[0] }}
                                                    </div>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label for="exampleEmail11" class="">Leave Type </label>

                                                    <select
                                                            name="select"
                                                            id="department"
                                                            v-model="form_data.leave_type"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': update_leave_error && update_leave_error.leave_type }"
                                                    >
                                                        <option v-for="leavetype in all_leaveTypes" :key="leavetype.id"
                                                                :value="leavetype.id">
                                                            {{ leavetype.leave_type }}
                                                        </option>
                                                    </select>
                                                    <div
                                                            :class="{ 'invalid-feedback': update_leave_error && update_leave_error.leave_type }"
                                                            v-if="update_leave_error && update_leave_error.leave_type"
                                                    >
                                                        {{ update_leave_error.leave_type[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <div class="col-md-6" v-if="form_data.employee_name">
                                                <label> Employee Name </label>
                                                <input
                                                        placeholder=""
                                                        id="employee_name"
                                                        v-model="form_data.employee_name.username"
                                                        type="text"
                                                        class="form-control"
                                                        disabled
                                                />
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-4">
                                                <div class="position-relative form-group">
                                                    <label>Application Start Date </label>
                                                    <input
                                                            name="Start-date"
                                                            id="application_start_date"
                                                            v-model="form_data.application_start_date"
                                                            placeholder=""
                                                            type="date"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': update_leave_error && update_leave_error.application_start_date }"
                                                            disabled
                                                    />
                                                    <div
                                                            :class="{ 'invalid-feedback': update_leave_error && update_leave_error.application_start_date }"
                                                            v-if="update_leave_error && update_leave_error.application_start_date"
                                                    >
                                                        {{ update_leave_error.application_start_date[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <div class="col-md-4 position-relative form-group">
                                                <label>Application End Date</label>
                                                <input
                                                        name="End-date"
                                                        id="application_end_date"
                                                        v-model="form_data.application_end_date"
                                                        placeholder="12/11/2020"
                                                        type="date"
                                                        class="form-control"
                                                        :class="{ 'is-invalid': update_leave_error && update_leave_error.application_end_date }"
                                                        disabled
                                                />
                                                <div
                                                        :class="{ 'invalid-feedback': update_leave_error && update_leave_error.application_end_date }"
                                                        v-if="update_leave_error && update_leave_error.application_end_date"
                                                >
                                                    {{ update_leave_error.application_end_date[0] }}
                                                </div>
                                            </div>

                                            <div class="col-md-4">
                                                <div class="position-relative form-group">
                                                    <label for="exampleEmail11" class="">Apply Day</label>
                                                    <input placeholder="2" id="apply_day_count"
                                                           v-model="form_data.apply_day_count" type="text"
                                                           class="form-control" disabled/>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-4">
                                                <div class="position-relative form-group">
                                                    <label for="exampleEmail11" class="">Approved Start Date </label>
                                                    <input
                                                            name="Start-date"
                                                            id="Start-date"
                                                            v-model="form_data.approved_start_date"
                                                            placeholder=""
                                                            type="date"
                                                            class="form-control"
                                                            :class="{ 'is-invalid': update_leave_error && update_leave_error.approved_start_date }"
                                                    />
                                                    <div
                                                            :class="{ 'invalid-feedback': update_leave_error && update_leave_error.approved_start_date }"
                                                            v-if="update_leave_error && update_leave_error.approved_start_date"
                                                    >
                                                        {{ update_leave_error.approved_start_date[0] }}
                                                    </div>
                                                </div>
                                            </div>

                                            <div class="col-md-4 position-relative form-group">
                                                <label>Approved End Date</label>
                                                <input
                                                        name="End-date"
                                                        id="End-date"
                                                        v-model="form_data.approved_end_date"
                                                        placeholder=""
                                                        type="date"
                                                        class="form-control"
                                                        :class="{ 'is-invalid': update_leave_error && update_leave_error.approved_end_date }"
                                                />
                                                <div
                                                        :class="{ 'invalid-feedback': update_leave_error && update_leave_error.approved_end_date }"
                                                        v-if="update_leave_error && update_leave_error.approved_end_date"
                                                >
                                                    {{ update_leave_error.approved_end_date[0] }}
                                                </div>
                                            </div>
                                            <div class="col-md-4">
                                                <div class="position-relative form-group">
                                                    <label for="exampleEmail11" class="">Approved Day</label>
                                                    <input
                                                            placeholder="2"
                                                            id="department_name"
                                                            v-model="form_data.approve_day_count"
                                                            type="text"
                                                            class="form-control"
                                                            disabled
                                                    />
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label for="exampleEmail11" class="">Select Leave Status</label>
                                                    <select name="select" id="type"
                                                            v-model="form_data.select_leave_status" type="choice"
                                                            class="form-control">
                                                        <option value="Pending">Pending</option>
                                                        <option value="Approve">Approved</option>
                                                        <option value="Decline">Decline</option>
                                                    </select>
                                                </div>
                                            </div>
                                            <div class="col-md-6">
                                                <div class="position-relative form-group">
                                                    <label> Approved By </label>
                                                    <br/>
                                                    <input
                                                            placeholder="HR Admin"
                                                            id="department_name"
                                                            v-model="form_data.approved_by.username"
                                                            type="text"
                                                            class="form-control"
                                                            disabled
                                                    />
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-row">
                                            <div class="col-md-12">
                                                <div class="position-relative form-group">
                                                    <label for="leaveDesc" class=""> Leave Description </label>
                                                    <br/>
                                                    <ckeditor :editor="editor" v-model="form_data.leave_description"
                                                              :config="editorConfig"></ckeditor>
                                                </div>
                                            </div>
                                        </div>

                                        <div class="form-group">
                                            <button @submit.prevent="submitEditLeave"
                                                    class="mt-2 btn btn-success btn-lg float-right">Edit Leave
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
        name: "UpdateLeaveReq",
        components: {
            Master,
            ckeditor: CKEditor.component
        },
        data() {
            return {
                editor: ClassicEditor,

                form_data: null,
                all_leaveTypes: null,
                update_leave_error: {
                    leave_subject: null,
                    leave_type: null,
                    // select_leave_status: null,
                    leave_description: null,
                    application_start_date: null,
                    // application_end_date: null,
                    approved_start_date: null,
                    // approved_end_date: null,
                },
            };
        },
        created() {
            this.getLeaveData();
            this.loadLeave();
        },
        methods: {
            loadLeave: function () {
                axios.get("leave-types/").then((response) => {
                    this.all_leaveTypes = response.data.results;
                    console.log(this.all_leaveTypes);
                    console.log(response.status);
                });
            },

            getLeaveData: function () {
                axios.get(`leaves/${this.$route.params.id}/`).then(
                    (response) => {
                        this.form_data = response.data;
                        this.form_data.leave_type = this.form_data.leave_type.id;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
            },

            submitEditLeave() {
                const token = localStorage.getItem("token");
                axios
                    .put("leaves/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        leave_subject: this.form_data.leave_subject,
                        leave_description: this.form_data.leave_description,
                        leave_type: this.form_data.leave_type,
                        select_leave_status: this.form_data.select_leave_status,
                        application_start_date: this.form_data.application_start_date,
                        application_end_date: this.form_data.application_end_date,
                        approved_start_date: this.form_data.approved_start_date,
                        approved_end_date: this.form_data.approved_end_date,
                        apply_day_count: this.form_data.apply_day_count,
                        approve_day_count: this.approve_day_count,
                    })

                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit a leave req..",
                        }).then((response) => {
                            this.$router.push({name: "AllLeaves"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.update_leave_error = error.response.data;
                        console.log(" updated error", error.response);
                    });
            },
        },
    };
</script>

<style scoped></style>
