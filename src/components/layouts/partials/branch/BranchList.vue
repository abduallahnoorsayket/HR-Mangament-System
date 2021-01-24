<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Branch List
                            </div>

                            <router-link :to="{ path: '/create-branch' }">
                                <a class="float" style="position: absolute; right: 0">
                                    <i class="fas fa-plus-square" style="color: #72e3da; font-size: 33px"></i>
                                </a>
                            </router-link>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-striped">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Branch Name</th>
                                        <th scope="col">Branch Address</th>
                                        <th scope="col">Branch Timezone</th>
                                        <th scope="col">Weekly Holidays</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(branch, SL) in branches" :key="branch.id">
                                        <!--                                    <td>{{branch.id}}</td>-->
                                        <td>{{ SL + 1 }}</td>

                                        <td>{{ branch.branch_name }}</td>
                                        <td>{{ branch.branch_address }}</td>
                                        <td>{{ branch.branch_timezone }}</td>
                                        <td>{{ branch.branch_weekly_holiday + " "}}</td>

                                        <td class="td_action">
                                            <!--                                    <button type="button" class="btn btn-primary">-->
                                            <!--                                        <i class="fas fa-eye"></i>-->
                                            <!--                                    </button>-->

                                            <!--                                            <button type="button" class="btn btn-success">-->
                                            <router-link class="item"
                                                         :to="{ name: 'UpdateBranch', params: { id: branch.id } }">
                                                <i class="fas fa-edit" style="color: #72e3da"></i>
                                            </router-link>
                                            <!--                                            </button>-->

                                            <!--                                            <button type="button" class="btn btn-danger">-->
                                            <i class="fas fa-trash-alt" style="color: firebrick; padding: 10px"
                                               @click="deleteBranchItem(branch.id) in branches"> </i>
                                            <!--                                            </button>-->
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
    import Master from "../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "BranchList",
        components: {Master},
        data() {
            return {
                branches: null,
            };
        },
        async created() {
            await this.GetBranchList();
        },
        methods: {
            GetBranchList: function () {
                axios.get("branches/").then((response) => {
                    this.branches = response.data;
                    console.log(this.branches);
                    console.log(response.status);
                });
            },

            deleteBranchItem: function (id) {
                Swal.fire({
                    title: "Are you sure?",
                    text: "You won't be able to revert this!",
                    icon: "warning",
                    showCancelButton: true,
                    confirmButtonColor: "#3085d6",
                    cancelButtonColor: "#d33",
                    confirmButtonText: "Yes, delete it!",
                }).then((response) => {
                    if (response.isConfirmed) {
                        axios.delete("branches/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetBranchList();
                            }
                        });
                        Swal.fire("Deleted!", "Your branch has been deleted!!", "success");
                    } else {
                        Swal.fire("Cancelled", "Your branch has not been deleted !", "error");
                    }
                });
            },
        },
    };
</script>

<style scoped>
    .table {
        text-align: center;
    }

    .td_action button {
        margin: 3px;
    }
</style>
