<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-user-plus icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Update Holiday
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">
                        <form @submit.prevent="EditHoliday">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Branch</label>
                                            <select
                                                    name="select[]"
                                                    v-model="branch"
                                                    class="form-control"
                                                    :class="{ 'is-invalid': create_holiday_error && create_holiday_error.branch }">

                                                <option value="false" disabled selected>Select Branch</option>
                                                <option v-for="branches in all_branches" :key="branches.id"
                                                        :value="branches.id">
                                                    {{ branches.branch_name }}
                                                </option>
                                            </select>
                                            <div :class="{ 'invalid-feedback': create_holiday_error && create_holiday_error.branch }"
                                                 v-if="create_holiday_error && create_holiday_error.branch">
                                                {{ create_holiday_error.branch[0] }}
                                            </div>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Holiday</label>
                                            <select
                                                    name="select[]"
                                                    v-model="holiday"
                                                    class="form-control"
                                                    :class="{ 'is-invalid': create_holiday_error && create_holiday_error.holiday }">

                                                <option value="false" disabled selected>Select Holiday</option>
                                                <option v-for="holiday in all_holidays" :key="holiday.id"
                                                        :value="holiday.id">
                                                    {{ holiday.purpose }}
                                                </option>
                                            </select>
                                            <div :class="{ 'invalid-feedback': create_holiday_error && create_holiday_error.holiday }"
                                                 v-if="create_holiday_error && create_holiday_error.holiday">
                                                {{ create_holiday_error.holiday[0] }}
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <div class="form-row">

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Start Date </label>
                                            <input
                                                    name="Start-date[]"
                                                    type="date"
                                                    v-model="start_date"
                                                    class="form-control"
                                                    :class="{ 'is-invalid': create_holiday_error && create_holiday_error.start_date }">

                                            <div :class="{ 'invalid-feedback': create_holiday_error && create_holiday_error.start_date }"
                                                 v-if="create_holiday_error && create_holiday_error.start_date">
                                                {{ create_holiday_error.start_date[0] }}
                                            </div>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <label>End Date</label>
                                        <input
                                                name="End-date[]"
                                                id="date"
                                                v-model="end_date"
                                                type="date"
                                                class="form-control"
                                                :class="{ 'is-invalid': create_holiday_error && create_holiday_error.end_date }">
                                        <div :class="{ 'invalid-feedback': create_holiday_error && create_holiday_error.end_date }"
                                             v-if="create_holiday_error && create_holiday_error.end_date">
                                            {{ create_holiday_error.end_date[0] }}
                                        </div>

                                    </div>
                                </div>
                            </div>

                            <div class="form-group">

                                <button @submit.prevent="EditHoliday"
                                        class="mt-2 btn btn-success btn-lg float-right">Submit
                                </button>
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
        name: "UpdateHoliday",
        components: {Master},
        data() {
            return {

                all_branches: null,
                all_holidays: null,

                branch: null,
                holiday: null,
                start_date: null,
                end_date: null,

                create_holiday_error: {
                    branch: null,
                    holiday: null,
                    start_date: null,
                    end_date: null,
                }

            };
        },

        methods: {
            getHolidayData: function () {
                axios.get(`branch-holidays/${this.$route.params.id}/`).then(
                    (response) => {
                        this.branch = response.data.branch.id;
                        this.holiday = response.data.holiday.id;
                        this.start_date = response.data.start_date;
                        this.end_date = response.data.end_date;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
            },

            loadBranchName() {
                const token = localStorage.getItem("token");
                axios
                    .get("branches/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_branches = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            loadHoliday() {
                const token = localStorage.getItem("token");
                axios
                    .get("holidays/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_holidays = response.data.results;
                        // this.pushReuseForm()
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            EditHoliday() {
                const token = localStorage.getItem("token");
                axios
                    .put("branch-holidays/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        branch: this.branch,
                        holiday: this.holiday,
                        start_date: this.start_date,
                        end_date: this.end_date,

                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit  Holiday..",
                        }).then((response) => {
                            // this.$router.push("branch-holiday");
                            this.$router.push({name: "Holiday"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.create_holiday_error = error.response.data;
                        console.log("--++", error.response);
                    });
            },


            // Remove Data Field Row
            removeReuseForm(index) {
                this.holiday_branch_holiday.splice(index, 1)

            },
        },

        created() {
            this.getHolidayData();
            this.loadBranchName();
            this.loadHoliday();
        },
    };
</script>

<style>

</style>
