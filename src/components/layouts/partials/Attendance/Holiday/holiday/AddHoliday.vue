<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i
                                        class="fas fa-user-plus icon-gradient bg-tempting-azure"
                                ></i>
                            </div>
                            <div>
                                Add Holiday
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="submitHolidayForm">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Purpose</label>
                                            <input
                                                    placeholder="name"
                                                    id="department_name"
                                                    v-model="purpose"
                                                    type="text"
                                                    class="form-control"
                                                    :class="{'is-invalid':error_add_holiday && error_add_holiday.purpose}"/>

                                            <!--error handling-->
                                            <div :class="{'invalid-feedback':error_add_holiday && error_add_holiday.purpose}"
                                                 v-if="error_add_holiday && error_add_holiday.purpose">
                                                {{error_add_holiday.purpose[0] }}
                                            </div>

                                        </div>
                                    </div>

                                    <div class="col-md-6 position-relative form-group">
                                        <label>Holiday Type</label>
                                        <select
                                                name="select"
                                                id="type"
                                                v-model="type"
                                                type="choice"
                                                class="form-control"
                                                :class="{ 'is-invalid': error_add_holiday && error_add_holiday.type }">
                                            <option value="PRI">Private</option>
                                            <option value="PUB">Public</option>
                                            <option value="OTH">Others</option>
                                        </select>
                                        <div :class="{ 'invalid-feedback': error_add_holiday && error_add_holiday.type }"
                                             v-if="error_add_holiday && error_add_holiday.type">
                                            {{ error_add_holiday.type[0] }}
                                        </div>
                                    </div>
                                </div>

                                <div class="form-group">
                                    <button @submit.prevent="submitHolidayForm"
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
    import Master from "../../../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "AddHoliday",
        components: {Master},
        data() {
            return {
                purpose: this.purpose,
                type: this.type,

                error_add_holiday: {
                    purpose: this.purpose,
                    type: this.type,
                }

            };
        },
        methods: {
            async submitHolidayForm() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("holidays/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        purpose: this.purpose,
                        type: this.type,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully create a holiday..",
                        }).then((result) => {
                            this.$router.push("holiday-list");
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.error_add_holiday = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
            },
        },
    };
</script>

<style scoped>
</style>