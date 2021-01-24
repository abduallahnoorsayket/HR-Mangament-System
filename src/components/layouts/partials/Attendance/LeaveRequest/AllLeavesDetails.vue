<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-eye icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div> All Leaves Details
                                <!-- <div class="page-title-subheading">Admin can create new user from this form.
                                </div> -->
                            </div>

                            <router-link :to="{path: '/all-leaves'}">
                                <a class="float" style="position: absolute; right: 0">
                                    <i class="fas fa-arrow-alt-circle-left"
                                       style="color: #72e3da; font-size: 33px"></i>
                                </a>
                            </router-link>

                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <div class="form-group" v-if="leaves">
                            <div class="form-row">
                                <div class="col-md-6">
                                    <div class="position-relative form-group">
                                        <div class="position-relative form-check">
                                            <div class="row">
                                                <div class="col-md-6" v-if="leaves.employee_name">
                                                    <b>{{"Name: " + leaves.employee_name.first_name + " " +
                                                        leaves.employee_name.last_name }}</b>
                                                    <br><br>
                                                    <p>{{ "Leave Type: " + leaves.leave_type.leave_type }}</p>
                                                    <p>{{ "Leave Description: " + leaves.leave_description }}</p>
                                                    <p>{{ "Leave Status: " + leaves.leave_type.leave_status}}</p>
                                                </div>
                                                <div class="col-md-6">

                                                    <p>{{ "Leave Duration: " + leaves.leave_type.duration + "days"}}</p>
                                                    <p>{{ "Leave Duration Type: " +
                                                        leaves.leave_type.duration_type}}</p>
                                                    <p>{{ "Leave Payable Wages: " +
                                                        leaves.leave_type.payable_wages}}</p>
                                                    <p>{{"Start Date: " + leaves.application_start_date }}</p>
                                                    <p>{{"End Date: " + leaves.application_end_date}}</p>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
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

    export default {
        name: "AllLeavesDetails",
        components: {Master},
        data() {
            return {
                leaves: null,
            }
        },


        created() {
            axios.get(`leaves/${this.$route.params.id}/`, {}).then((response) => {
                this.leaves = response.data;
                console.log('####', this.leaves);
            });
        },
    }
</script>


<style scoped>

</style>