<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-edit icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Update Leave Types
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">
                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="EditLeaveType">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Leave Type</label>
                                            <input
                                                    placeholder="leave type"
                                                    id="leave-type"
                                                    type="text"
                                                    v-model="leave_type"
                                                    class="form-control"
                                                    :class="{'is-invalid':update_leave_error && update_leave_error.leave_type}"/>

                                            <!--error handling-->
                                            <div :class="{'invalid-feedback':update_leave_error && update_leave_error.leave_type}"
                                                 v-if="update_leave_error && update_leave_error.leave_type">
                                                {{update_leave_error.leave_type[0] }}
                                            </div>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Select Leave Status</label>
                                            <select
                                                    name="select"
                                                    id="leave-status"
                                                    v-model="leave_status"
                                                    type="boolean"
                                                    class="form-control"
                                                    :class="{'is-invalid':update_leave_error && update_leave_error.leave_status}">
                                                <option>Paid</option>
                                                <option>Unpaid</option>
                                                <span>leave_status: {{ leave_status }}</span>

                                                <!--error handling-->
                                                <div :class="{'invalid-feedback':update_leave_error && update_leave_error.leave_status}"
                                                     v-if="update_leave_error && update_leave_error.leave_status">
                                                    {{update_leave_error.leave_status[0] }}
                                                </div>
                                            </select>
                                        </div>
                                    </div>
                                </div>

                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Duration</label>
                                            <input
                                                    placeholder="duration"
                                                    id="duration"
                                                    v-model="duration"
                                                    type="number" name="quantity" min="1" max="365"
                                                    class="form-control"
                                                    :class="{'is-invalid':update_leave_error && update_leave_error.duration}"/>

                                            <!--error handling-->
                                            <div :class="{'invalid-feedback':update_leave_error && update_leave_error.duration}"
                                                 v-if="update_leave_error && update_leave_error.duration">
                                                {{update_leave_error.duration[0] }}
                                            </div>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Select Duration Type</label>
                                            <select
                                                    name="select"
                                                    id="duration-type"
                                                    v-model="duration_type"
                                                    type="boolean"
                                                    class="form-control"
                                                    :class="{'is-invalid':update_leave_error && update_leave_error.duration_type}">
                                                <option>Day</option>
                                                <option>Night</option>
                                                <span>duration_type: {{ duration_type }}</span>

                                                <!--error handling-->
                                                <div :class="{'invalid-feedback':update_leave_error && update_leave_error.duration_type}"
                                                     v-if="update_leave_error && update_leave_error.duration_type">
                                                    {{update_leave_error.duration_type[0] }}
                                                </div>
                                            </select>
                                        </div>
                                    </div>
                                </div>

                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <div>
                                                <label>Payable Wages</label>
                                            </div>
                                            <input
                                                    placeholder="payable wages"
                                                    id="payable-wages"
                                                    type="text"
                                                    v-model="payable_wages"
                                                    class="form-control"
                                                    :class="{'is-invalid':update_leave_error && update_leave_error.payable_wages}"/>

                                            <!--error handling-->
                                            <div :class="{'invalid-feedback':update_leave_error && update_leave_error.payable_wages}"
                                                 v-if="addupdate_leave_error_leave_error && update_leave_error.payable_wages">
                                                {{update_leave_error.payable_wages[0] }}
                                            </div>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Branch Name</label>
                                            <select
                                                    name="select"
                                                    id="branch"
                                                    v-model="branch"
                                                    class="form-control"
                                                    :class="{'is-invalid':update_leave_error && update_leave_error.branch}">

                                                <option value="false" disabled selected>Select Branch</option>
                                                <option v-for="branch in branch_name" :key="branch.id" :value="branch.id">
                                                    {{ branch.branch_name }}
                                                </option>

                                                <!--error handling-->
                                                <div :class="{'invalid-feedback':update_leave_error && update_leave_error.branch}"
                                                     v-if="update_leave_error && update_leave_error.branch">
                                                    {{update_leave_error.branch[0] }}
                                                </div>
                                            </select>
                                        </div>
                                    </div>
                                </div>

                                <div class="position-relative form-group">
                                    <div>
                                        <label>Remarks</label>
                                    </div>
                                    <textarea
                                            name="Remarks"
                                            id="Remarks"
                                            placeholder="Remarks"
                                            type="text"
                                            v-model="remarks"
                                            class="form-control"
                                            rows="4"
                                            cols="50">

                                    </textarea>

                                </div>


                                <div class="form-group">
                                    <button @submit.prevent="EditLeaveType"
                                            class="mt-2 btn btn-success btn-lg float-right">
                                        Submit
                                    </button>
                                </div>
                            </div>
                        </form>
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

    export default {
        name: "UpdateLeaveTypes",
        components: {Master},
        data() {
            return {
                leave_type: null,
                leave_status: null,
                duration: null,
                duration_type: null,
                payable_wages: null,
                remarks: null,
                branch: null,

                branch_name: null,

                update_leave_error: {
                    leave_type: null,
                    leave_status: null,
                    duration: null,
                    duration_type: null,
                    payable_wages: null,
                    remarks: null,
                    branch: null,
                }
            };
        },
        methods: {

            loadBranchName() {
                const token = localStorage.getItem("token");
                axios
                    .get("branches/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.branch_name = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            getLeaveTypeData: function () {
                axios.get(`leave-types/${this.$route.params.id}/`).then(
                    (response) => {
                        this.leave_type = response.data.leave_type;
                        this.leave_status = response.data.leave_status;
                        this.duration = response.data.duration;
                        this.duration_type = response.data.duration_type;
                        this.payable_wages = response.data.payable_wages;
                        this.remarks = response.data.remarks;
                        this.branch = response.data.branch.id;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
            },

            EditLeaveType() {
                const token = localStorage.getItem("token");
                axios
                    .put("leave-types/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        leave_type: this.leave_type,
                        leave_status: this.leave_status,
                        duration: this.duration,
                        duration_type: this.duration_type,
                        payable_wages: this.payable_wages,
                        remarks: this.remarks,
                        branch: this.branch,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit a leave type..",
                        }).then((response) => {
                            this.$router.push({name: "LeaveTypes"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.update_leave_error = error.response.data;
                        console.log("--++", error.response);
                    });
            },
        },
        created() {
            this.getLeaveTypeData();
            this.loadBranchName();
        },
    };
</script>

<style scoped>
</style>