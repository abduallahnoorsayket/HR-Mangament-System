<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="far fa-plus-square icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Create Branch Holidays
                            </div>

                        </div>
                    </div>

                </div>


                <div class="row mt-5">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <form @submit.prevent="submitCreateHoliday">
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
                                                        <option v-for="branches in all_branch" :key="branches.id"
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

                                        <div class="form-group">

                                            <button @submit.prevent="submitCreateHoliday"
                                                    class="mt-2 btn btn-success btn-lg float-right">Submit
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

    export default {
        name: "CreateHoliday",
        components: {Master},
        data() {
            return {

                all_branch: null,

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
            }
        },

        methods: {
            // Submit Holiday Form
            async submitCreateHoliday() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("branch-holidays/", {
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
                            text: "You have successfully Added  Holiday..",
                        }).then((result) => {
                            this.$router.push("branch-holiday");
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.create_holiday_error = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
            },

            // Load branch into dropdown
            loadBranch() {
                const token = localStorage.getItem("token");
                axios
                    .get("branches/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_branch = response.data;
                        // this.pushReuseForm()
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

            // Add Data Field Row
            pushReuseForm() {
                this.holiday_branch_holiday.push({
                    branch: null,
                    start_date: null,
                    end_date: null,
                    holiday: null,
                })
            },

            // Remove Data Field Row
            removeReuseForm(index) {
                this.holiday_branch_holiday.splice(index, 1)

            },
        },


        created() {
            this.loadBranch();
            this.loadHoliday();
        }
    }
</script>

<style scoped>

</style>