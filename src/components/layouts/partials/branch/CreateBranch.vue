<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-plus-square icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Create Branch

                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">

                        <form @submit.prevent="submitBranchForm">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Branch Name</label>
                                            <input
                                                    placeholder="name"
                                                    id="department_name"
                                                    v-model="branch_name"
                                                    type="text"
                                                    class="form-control"
                                                    :class="{ 'is-invalid': branch_error_data && branch_error_data.branch_name }"/>

                                            <div
                                                    :class="{ 'invalid-feedback': branch_error_data && branch_error_data.branch_name }"
                                                    v-if="branch_error_data && branch_error_data.branch_name"
                                            >
                                                {{ branch_error_data.branch_name[0] }}
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
                                                    class="form-control"
                                                    rows="1"
                                                    cols="50"
                                            />
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
                                        <div class="position-relative form-group">
                                            <label>Branch Address</label>

                                            <textarea
                                                    name="short_bio"
                                                    id="branch_address"
                                                    v-model="branch_address"
                                                    placeholder="address"
                                                    type="text"
                                                    class="form-control"
                                                    rows="1"
                                                    cols="50"
                                                    :class="{ 'is-invalid': branch_error_data && branch_error_data.branch_address }"/>

                                            <div :class="{ 'invalid-feedback': branch_error_data && branch_error_data.branch_address }"
                                                 v-if="branch_error_data && branch_error_data.branch_address">
                                                {{ branch_error_data.branch_address[0] }}
                                            </div>
                                        </div>
                                    </div>
                                </div>



                                <!--Timezone Dropdown with Search-->
                               <!-- <input class="form-control" list="europe-countries" placeholder="Select timezone" v-model="branch_timezone">
                                <datalist id="europe-countries">
                                    <option v-for="timezone in timezones" :key="timezone" :value="timezone"> {{timezone}} </option>
                                </datalist>-->
                                <!--Timezone Dropdown with Search-->



                                <div class="form-group">
                                    <button @submit.prevent="submitBranchForm"
                                            class="mt-2 btn btn-success btn-lg float-right">Submit
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
        name: "CreateBranch",
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
                    branch_timezone: null,
                },
            };
        },
        methods: {
            async submitBranchForm() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("branches/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        branch_name: this.branch_name,
                        branch_address: this.branch_address,
                        branch_timezone: this.branch_timezone,
                        branch_weekly_holiday: this.branch_weekly_holiday ? this.branch_weekly_holiday.split(",") : [],
                    })
                    .then((response) => {
                        if (response.status === 201) {
                            Swal.fire({
                                icon: "success",
                                // title: "Yes...",
                                text: "You have successfully create a branch..",
                            });
                            this.$router.push("branch-list");
                            // console.log(response);
                        }
                    })
                    .catch((error) => {
                        this.branch_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
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
                        console.log('========', response)
                        this.timezones = response.data;
                        // this.pushReuseForm()
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
        },

        created() {
            this.loadTimezone();
        }
    };
</script>

<style scoped></style>
