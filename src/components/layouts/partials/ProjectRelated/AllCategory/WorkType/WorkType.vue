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
                            <div> Work Type </div>

                            <a class="float" style="position: absolute; right: 0">
                                <button
                                        @click="OpenModal()"
                                        type="button"
                                        class="btn btn-success"
                                        style="font-size: 15px">
                                    <span class="fas fa-plus-square"></span>
                                    Add Work Type
                                </button>
                            </a>

                            <!--<router-link :to="{path: '/create-group'}">
                                <a class="float" style="position: absolute; right: 0">
                                    <i class="fas fa-plus-square" style="color: #72e3da; font-size: 33px"></i>
                                </a>
                            </router-link>-->
                        </div>
                    </div>
                </div>

                <!--AddWorkTypeModal Component Calling-->
                <AddWorkTypeModal
                        @close-modal="CloseModal"
                        v-if="isModalVisible"
                        :employee_id="{ isModalVisible: isModalVisible,}"
                />

                <!--UpdateWorkTypeModal Component Calling-->
                <UpdateWorkTypeModal @close-update="CloseUpdateModal" v-if="isUpdateVisible" :employee_update="{
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
                                        <th scope="col">Work Type Name</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(work, SL) in work_type" :key="work.id">
                                        <td>{{SL+1}}</td>
                                        <td>{{work.name}}</td>
                                        <td class="td_action">

                                            <!--<router-link class="item"
                                                         :to="{ name: 'GroupDetails', params: { id: group.id } }">
                                                <i class="fas fa-eye" style="color: #72e3da; padding: 10px"></i>
                                            </router-link>-->


                                            <i class="fas fa-edit" style="color: #00c6ff"
                                               @click="OpenUpdateModal(work.id)"></i>


                                            <i class="fas fa-trash-alt" style="color: #b71c1c; padding: 10px"
                                               @click="deleteWorkTypeItem(work.id) in work_type">
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
    import AddWorkTypeModal from "./AddWorkTypeModal";
    import UpdateWorkTypeModal from "./UpdateWorkTypeModal";

    export default {
        name: "WorkType",
        components: {Master, AddWorkTypeModal, UpdateWorkTypeModal},
        data() {
            return {
                work_type: null,
                isModalVisible: false,
                update_attendance_id: null,
                isUpdateVisible: false,
            };
        },
        async created() {
            await this.getWorkTypeList();
        },
        methods: {
            getWorkTypeList: function () {
                axios.get("work-types/").then((response) => {
                    this.work_type = response.data;
                    console.log(this.work_type);
                    console.log(response.status);
                });
            },

            deleteWorkTypeItem: function (id) {
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
                        axios.delete("work-types/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.getWorkTypeList();
                            }
                            Swal.fire("Deleted!", "Your work type has been deleted!!", "success");
                        });
                        // Swal.fire("Deleted!", "Your group has been deleted!!", "success");
                        Swal.fire("Cancelled", "Your work type has not been deleted !", "error");
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