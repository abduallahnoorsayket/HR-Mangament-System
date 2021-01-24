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
                                Update Branch
                                <!-- <div class="page-title-subheading">Admin can update department from this form.
                                            </div> -->
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">
                        <h5 class="card-title">Update Branch</h5>

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="EditBranch">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Name</label>
                                            <input
                                                    placeholder="department name"
                                                    id="branch_name"
                                                    v-model="branch_name"
                                                    type="text"
                                                    class="form-control"
                                                    :class="{'is-invalid':branch_error_data && branch_error_data.branch_name}"/>

                                            <div :class="{'invalid-feedback':branch_error_data && branch_error_data.branch_name}"
                                                 v-if="branch_error_data && branch_error_data.branch_name">{{
                                                branch_error_data.branch_name[0] }}
                                            </div>

                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Weekly Holidays</label>

                                            <input
                                                    name="short_bio"
                                                    id="branch_weekly_holiday"
                                                    v-model="branch_weekly_holiday"
                                                    placeholder="Holidays"
                                                    type="text"
                                                    class="form-control"/>
                                        </div>
                                    </div>
                                </div>

                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Branch Timezone</label>
                                            <select
                                                    name="select[]"
                                                    v-model="branch_timezone"
                                                    class="form-control"
                                                    >

                                                <option value="false" disabled selected>Select Timezone</option>
                                                <option v-for="timezone in timezones" :key="timezone"
                                                        :value="timezone">
                                                    {{ timezone }}
                                                </option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <label>Address</label>
                                        <textarea
                                                name="branch_address"
                                                v-model="branch_address"
                                                placeholder="address"
                                                type="text"
                                                class="form-control"
                                                rows="1"
                                                cols="50"
                                                :class="{'is-invalid':branch_error_data && branch_error_data.branch_address}"/>

                                        <div :class="{'invalid-feedback':branch_error_data && branch_error_data.branch_address}"
                                             v-if="branch_error_data && branch_error_data.branch_address">{{
                                            branch_error_data.branch_address[0] }}
                                        </div>
                                    </div>
                                </div>

                                <div class="form-group">
                                    <button @submit.prevent="EditBranch"
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
    import Master from "../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "UpdateBranch",
        components: {Master},
        data() {
            return {
                branch_name: null,
                branch_address: null,
                branch_timezone: null,
                branch_weekly_holiday: [],

                timezones: null,

                branch_error_data: {
                    branch_name: null,
                    branch_address: null,
                }
            };
        },
        methods: {
            getBranchData: function () {
                axios.get(`branches/${this.$route.params.id}/`).then(
                    (response) => {
                        this.branch_name = response.data.branch_name;
                        this.branch_address = response.data.branch_address;
                        this.branch_timezone = response.data.branch_timezone;
                        this.branch_weekly_holiday = response.data.branch_weekly_holiday;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
            },

            EditBranch() {
                const token = localStorage.getItem("token");
                axios
                    .put("branches/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        branch_name: this.branch_name,
                        branch_address: this.branch_address,
                        branch_timezone: this.branch_timezone,
                        branch_weekly_holiday: this.branch_weekly_holiday ? this.branch_weekly_holiday.split(",") : [],
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit a branch..",
                        }).then((response) => {
                            this.$router.push({name: "BranchList"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.branch_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
            },


            loadTimezone() {
                const token = localStorage.getItem("token");
                axios
                    .get("timezones/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.timezones = response.data;
                        // this.pushReuseForm()
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
        },
        created() {
            this.getBranchData();
            this.loadTimezone();
        },
    };
</script>

<style scoped>
</style>