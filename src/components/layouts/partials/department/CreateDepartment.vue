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
                                Create Department
                                <!-- <div class="page-title-subheading">Admin can create new user from this form.
                                            </div> -->
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="submitDepartmentForm">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Department Name</label>
                                            <input
                                                    placeholder="name"
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
                                        <label>Department Code</label>
                                        <input
                                                placeholder="code"
                                                id="department_code"
                                                v-model="department_code"
                                                type="text"
                                                class="form-control"
                                                :class="{'is-invalid':department_error_data && department_error_data.department_code}"/>

                                        <!--error handling-->
                                        <div :class="{'invalid-feedback':department_error_data && department_error_data.department_code}"
                                             v-if="department_error_data && department_error_data.department_code">
                                            {{department_error_data.department_code[0] }}
                                        </div>
                                    </div>
                                </div>

                                <div class="form-group">
                                    <button @submit.prevent="submitDepartmentForm"
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
        name: "CreateDepartment",
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
            async submitDepartmentForm() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("departments/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        department_name: this.department_name,
                        department_code: this.department_code,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes..."
                            text: "You have successfully create a department..",
                        }).then((result) => {
                            this.$router.push("department-list");
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.department_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
            },
        },
    };
</script>

<style scoped>
</style>