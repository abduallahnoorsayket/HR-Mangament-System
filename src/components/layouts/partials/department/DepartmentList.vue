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
                                Department List
                            </div>

                            <router-link :to="{path: '/create-department'}">
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
                                        <th scope="col">Department Name</th>
                                        <th scope="col">Department Code</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(department, SL) in departments" :key="department">
                                        <td>{{SL+1}}</td>
                                        <td>{{department.department_name}}</td>
                                        <td>{{department.department_code}}</td>

                                        <td class="td_action">

                                            <router-link class="item"
                                                         :to="{ name: 'UpdateDepartment', params: { id: department.id } }">
                                                <i class="fas fa-edit" style="color: #72e3da"></i>
                                            </router-link>

                                            <i class="fas fa-trash-alt" style="color: firebrick; padding: 10px"
                                               @click="deleteDepartmentItem(department.id) in departments">
                                            </i>
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
        name: "DepartmentList",
        components: {Master},
        data() {
            return {
                departments: null,
            };
        },
        async created() {
            await this.GetDepartmentList();
        },
        methods: {
            GetDepartmentList: function () {
                axios.get("departments/").then((response) => {
                    this.departments = response.data;
                    console.log(this.departments);
                    console.log(response.status);
                });
            },

            deleteDepartmentItem: function (id) {
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
                        axios.delete("departments/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetDepartmentList();
                            }
                        });
                        Swal.fire(
                            "Deleted!",
                            "Your department has been deleted!!",
                            "success"
                        );
                    } else {
                        Swal.fire(
                            "Cancelled",
                            "Your department has not been deleted !",
                            "error"
                        );
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