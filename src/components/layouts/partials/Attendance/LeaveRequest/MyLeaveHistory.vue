<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-history icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                My Leave History
                            </div>
                        </div>
                    </div>

                </div>
                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-striped ">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Types of Leave</th>
                                        <th scope="col">Total</th>
                                        <th scope="col">Total Taken</th>
                                        <th scope="col">Available</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(l_history, SL) in leave_history" :key="l_history.id">
                                        <td>{{ SL +1 }}</td>
                                        <td>{{ l_history.leave_type }}</td>
                                        <td>{{ l_history.duration }} days</td>
                                        <td>{{ l_history.employee_leave_count }} days</td>

                                        <td v-if="l_history.duration >= l_history.employee_leave_count">
                                            {{ l_history.duration - l_history.employee_leave_count }}
                                        </td>
                                    </tr>
                                    </tbody>
                                </table>
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
        name: "MyLeaveHistory",
        components: {Master},
        data() {
            return {
                leave_history: null
            }
        },
        async created() {
            await this.GetLeaveHistoryList();
        },
        methods: {
            GetLeaveHistoryList: function () {
                axios.get("leave-history/").then((response) => {
                    this.leave_history = response.data;
                    console.log(this.leave_history);
                    console.log(response.status);
                });
            },
        },

    }
</script>

<style scoped>

</style>