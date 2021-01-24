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
                                Group List
                            </div>

                            <router-link :to="{path: '/create-group'}">
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
                                <table class="table table-striped ">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Group Name</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(group, SL) in groups" :key="group.id">
                                        <td>{{SL+1}}</td>
                                        <td>{{group.name}}</td>
                                        <td class="td_action">

                                            <router-link class="item"
                                                         :to="{ name: 'GroupDetails', params: { id: group.id } }">
                                                <i class="fas fa-eye" style="color: #72e3da; padding: 10px"></i>
                                            </router-link>

                                            <router-link class="item"
                                                         :to="{ name: 'UpdateGroup', params: { id: group.id } }">
                                                <i class="fas fa-edit" style="color: #72e3da"></i>
                                            </router-link>


                                            <i class="fas fa-trash-alt" style="color: #b71c1c; padding: 10px"
                                               @click="deleteGroupItem(group.id) in groups">
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
        name: "GroupList",
        components: {Master},
        data() {
            return {
                groups: null,
            };
        },
        async created() {
            await this.GetUserList();
        },
        methods: {
            GetUserList: function () {
                axios.get("groups/").then((response) => {
                    this.groups = response.data;
                    console.log(this.groups);
                    console.log(response.status);
                });
            },

            deleteGroupItem: function (id) {
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
                        axios.delete("groups/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetUserList();
                            }
                            Swal.fire("Deleted!", "Your group has been deleted!!", "success");
                        });
                        // Swal.fire("Deleted!", "Your group has been deleted!!", "success");
                        Swal.fire("Cancelled", "Your group has not been deleted !", "error");
                    } else {
                        // Swal.fire("Cancelled", "Your group has not been deleted !", "error");
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