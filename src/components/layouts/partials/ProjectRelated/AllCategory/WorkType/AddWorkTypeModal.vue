<template>
    <teleport to="body">
        <div v-if="isModalVisible" class="modal">
            <div class="main-card card">
                <div class="card-header">
                    <p class="float-left">Add Work Type</p>
                </div>
                <div class="card-body" style="width: 600px">
                    <form @submit.prevent="submitWorkTypeForm">
                        <label>Work Type Name</label>
                        <input placeholder="Type Name" id="work_type_name" v-model="name" type="text"
                               class="form-control"/>

                        <!--Submit & Close Button-->
                        <div class="form-group">
                            <div class="form-row">
                                <div class="col-md-6"></div>
                                <div class="col-md-6">
                                    <button class="mt-2 float-right ml-2 btn btn-secondary" @click="CloseModal()">
                                        Close
                                    </button>
                                    <button @submit.prevent="submitWorkTypeForm"
                                            class="mt-2 btn btn-success float-right">
                                        Submit
                                    </button>
                                </div>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </teleport>
</template>

<script>
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "AddWorkTypeModal",
        props: ["employee_id"],
        data() {
            return {
                isModalVisible: false,

                name: null,
            };
        },
        created() {
            this.employee = this.$props.employee_id.employee;
            this.isModalVisible = this.$props.employee_id.isModalVisible;
        },

        methods: {
            CloseModal() {
                this.isModalVisible = !this.isModalVisible;
                this.$emit("close-modal", this.isModalVisible);
            },

            async submitWorkTypeForm() {
                axios
                    .post("work-types/", {
                        name: this.name,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            text: "You have successfully Added a Work Type ..",
                        }).then((result) => {
                            // this.$router.push("work-type");
                            this.$router.push({name: "WorkType"});
                            this.$router.go();
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.AddAttendance_error = error.response.data;
                        console.log("--++", error.response);
                    });
                // console.log(response);
            },

            // // Permissions
            // /*for Module(app)*/
            // hasModulePermission(...module_name) {
            //     return permissions.hasModulePermission(...module_name)
            //     // return permissions.hasModulePermission.apply(...module_name)
            // },
            //
            // /*for Model*/
            // hasModelPermission(model_name) {
            //     return permissions.hasModelPermission(model_name)
            // },
            //
            // /*for code_name*/
            // hasPermission(permission_name) {
            //     return permissions.hasPermission(permission_name)
            // },
        },
    };
</script>
<style>
    /* teleport modal */
    .modal {
        /* position: absolute; */
        top: 20;
        right: 0;
        bottom: 20;
        left: 0;
        background-color: rgba(0, 0, 0, 0.5);
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .btn-close {
        float: right !important;
        border: none;
        font-size: 20px;
        padding: 20px;
        cursor: pointer;
        font-weight: bold;
        color: #4aae9b;
        background: transparent;
    }
</style>