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
                            <div> Work Priority</div>
                            <a class="float" style="position: absolute; right: 0">
                                <button
                                        @click="OpenModal()"
                                        type="button"
                                        class="btn btn-success"
                                        style="font-size: 15px">
                                    <span class="fas fa-plus-square"></span>
                                    Add Work Priority
                                </button>
                            </a>
                        </div>
                    </div>
                </div>

                  <!--AddWorkTypeModal Component Calling-->
                <AddWorkPriorityModal
                        @close-modal="CloseModal"
                        v-if="isModalVisible"
                        :employee_id="{ isModalVisible: isModalVisible,}"
                />

                <!--UpdateWorkTypeModal Component Calling-->
                <UpdateWorkPriorityModal @close-update="CloseUpdateModal" v-if="isUpdateVisible" :employee_update="{
                    update_attendance_id: update_attendance_id,
                    isUpdateVisible: isUpdateVisible,
                  }" />

                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-striped ">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Work Priority</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(priority, SL) in work_priority" :key="priority.id">
                                        <td>{{SL+1}}</td>
                                        <td>{{priority.name}}</td>
                                        <td class="td_action">

                                            <i class="fas fa-edit" style="color: #00c6ff"
                                               @click="OpenUpdateModal(priority.id)"></i>


                                            <i class="fas fa-trash-alt" style="color: #b71c1c; padding: 10px"
                                               @click="deleteWorkPriorityItem(priority.id) in work_priority">
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
    import AddWorkPriorityModal from "./AddWorkPriorityModal";
    import UpdateWorkPriorityModal from "./UpdateWorkPriorityModal";

    export default {
        name: "WorkPriority",
        components: {Master, AddWorkPriorityModal, UpdateWorkPriorityModal},
        data() {
            return {
                work_priority: null,
                isModalVisible: false,
                update_attendance_id: null,
                isUpdateVisible: false,
            };
        },
        async created() {
            await this.getWorkPriorityList();
        },
        methods: {
            getWorkPriorityList: function () {
                axios.get("work-priorities/").then((response) => {
                    this.work_priority = response.data;
                    console.log(this.work_priority);
                    console.log(response.status);
                });
            },

            deleteWorkPriorityItem: function (id) {
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
                        axios.delete("work-priorities/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.getWorkPriorityList();
                            }
                            Swal.fire("Deleted!", "Your Work Priority has been deleted!!", "success");
                        });
                        // Swal.fire("Deleted!", "Your group has been deleted!!", "success");
                        Swal.fire("Cancelled", "Your Work Priority has not been deleted !", "error");
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