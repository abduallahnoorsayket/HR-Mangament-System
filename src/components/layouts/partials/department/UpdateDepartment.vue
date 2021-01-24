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
                                Update Department
                                <!-- <div class="page-title-subheading">Admin can update department from this form.
                                            </div> -->
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">
                        <h5 class="card-title">Update Department</h5>

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="EditDepartment">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Name</label>
                                            <input
                                                    placeholder="department name"
                                                    id="department_name"
                                                    v-model="department_name"
                                                    type="text"
                                                    class="form-control"
                                                    :class="{'is-invalid':department_error_data && department_error_data.department_name}"/>

                                             <!--error handling-->
                                            <div :class="{'invalid-feedback':department_error_data && department_error_data.department_name}"
                                                 v-if="department_error_data && department_error_data.department_name">
                                                {{department_error_data.department_name[0] }}
                                            </div>

                                        </div>
                                    </div>

                                    <div class="col-md-6 position-relative form-group">
                                        <label for="exampleEmail11" class=""
                                        >Department Code</label
                                        >
                                        <input
                                                placeholder="department code"
                                                id="department_code"
                                                v-model="department_code"
                                                type="text"
                                                class="form-control"
                                        />
                                    </div>
                                </div>

                                <div class="form-group">
                                    <button
                                            @submit.prevent="EditDepartment"
                                            class="mt-2 btn btn-success btn-lg float-right"
                                    >
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
        name: "UpdateDepartment",
        components: {Master},
        data() {
            return {
                department_name: null,
                department_code: null,

                department_error_data: {
                    department_name: null,
                    department_code: null,
                }
            };
        },
        methods: {
            getDepartmentData: function () {
                axios.get(`departments/${this.$route.params.id}/`).then(
                    (response) => {
                        this.department_name = response.data.department_name;
                        this.department_code = response.data.department_code;
                    },
                    (response) => {
                        console.log("----", response);
                    }
                );
            },

            EditDepartment() {
                const token = localStorage.getItem("token");
                axios
                    .put("departments/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        department_name: this.department_name,
                        department_code: this.department_code,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit a department..",
                        }).then((response) => {
                            this.$router.push({name: "DepartmentList"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.department_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
            },
        },
        created() {
            this.getDepartmentData();
        },
    };
</script>

<style scoped>
</style>