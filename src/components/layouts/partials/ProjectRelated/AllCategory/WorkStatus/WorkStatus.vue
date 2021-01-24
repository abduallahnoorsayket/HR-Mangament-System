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
                            <div> Work Status</div>

                            <a class="float" style="position: absolute; right: 0">
                                <button
                                        @click="OpenModal()"
                                        type="button"
                                        class="btn btn-success"
                                        style="font-size: 15px">
                                    <span class="fas fa-plus-square"></span>
                                    Add Work Status
                                </button>
                            </a>
                        </div>
                    </div>
                </div>


                <!--AddWorkStatusModal Component Define-->
                <AddWorkStatusModal
                        @close-modal="CloseModal"
                        v-if="isModalVisible"
                        :employee_id="{ isModalVisible: isModalVisible,}"
                />

                <!--UpdateWorkTypeModal Component Calling-->
                <UpdateWorkStatusModal @close-update="CloseUpdateModal" v-if="isUpdateVisible" :employee_update="{
                    update_attendance_id: update_attendance_id,
                    isUpdateVisible: isUpdateVisible,
                  }"/>


                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-striped ">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Work Status Name</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(status, SL) in work_status" :key="status.id">
                                        <td>{{SL+1}}</td>
                                        <td>{{status.name}}</td>
                                        <td class="td_action">

                                            <i class="fas fa-edit" style="color: #00c6ff"
                                               @click="OpenUpdateModal(status.id)"></i>


                                            <i class="fas fa-trash-alt" style="color: #b71c1c; padding: 10px"
                                               @click="deleteWorkStatusItem(status.id) in work_status">
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
    import Master from "../../../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";
    import AddWorkStatusModal from "./AddWorkStatusModal";
    import UpdateWorkStatusModal from "./UpdateWorkStatusModal";

    export default {
        name: "WorkStatus",
        components: {Master, AddWorkStatusModal, UpdateWorkStatusModal},
        data() {
            return {
                work_status: null,
                isModalVisible: false,
                update_attendance_id: null,
                isUpdateVisible: false,
            };
        },
        async created() {
            await this.getWorkStatusList();
        },
        methods: {
            getWorkStatusList: function () {
                axios.get("work-statuses/").then((response) => {
                    this.work_status = response.data;
                    console.log(this.work_status);
                    console.log(response.status);
                });
            },

            deleteWorkStatusItem: function (id) {
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
                        axios.delete("work-statuses/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.getWorkStatusList();
                            }
                            Swal.fire("Deleted!", "Your work status has been deleted!!", "success");
                        });
                        // Swal.fire("Deleted!", "Your group has been deleted!!", "success");
                        Swal.fire("Cancelled", "Your work status has not been deleted !", "error");
                    } else {
                        // Swal.fire("Cancelled", "Your group has not been deleted !", "error");
                    }
                });
            },

            OpenModal() {
                this.isModalVisible = !this.isModalVisible;
            },
            CloseModal(isModalVisible) {
                this.isModalVisible = isModalVisible;
            },
            OpenUpdateModal(id) {
                this.isUpdateVisible = !this.isUpdateVisible;
                this.update_attendance_id = id;
            },
            CloseUpdateModal(isUpdateVisible) {
                this.isUpdateVisible = isUpdateVisible;
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