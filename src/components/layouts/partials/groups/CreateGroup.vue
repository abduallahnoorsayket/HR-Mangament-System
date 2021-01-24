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
                            Create Groups
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body">

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="submitGroupForm">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label>Name</label>
                                            <input
                                                    placeholder="name"
                                                    id="name"
                                                    v-model="name"
                                                    type="text"
                                                    class="form-control"
                                                    :class="{'is-invalid':group_error_data && group_error_data.name}"/>

                                            <!--error handling-->
                                            <div :class="{'invalid-feedback':group_error_data && group_error_data.name}"
                                                 v-if="group_error_data && group_error_data.name">
                                                {{group_error_data.name[0] }}
                                            </div>
                                        </div>
                                    </div>


                                    <div class="col-md-6 permissions">
                                        <label>Permissions</label><br>
                                        <input type="text" v-model="searchQuery" placeholder="filter"
                                               style="outline: none;">
                                        <div class=" grp-per" v-if="all_permission">

                                            <div class="position-relative form-check"
                                                 v-for="prmsn in resultQuery"
                                                 :key="prmsn.id">
                                                <label class="form-check-label">
                                                    <input
                                                            type="checkbox"
                                                            :id="prmsn.id"
                                                            :value="prmsn.id"
                                                            v-model="permissions"
                                                            class="form-check-input"/>{{ prmsn.name }}
                                                </label>

                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <div class="form-group">
                                    <button
                                            @submit.prevent="submitGroupForm"
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
        name: "CreateGroup",
        components: {Master},
        data() {
            return {
                name: null,
                permissions: [],
                all_permission: null,
                searchQuery: null,

                group_error_data: {
                    name: null,
                }
            };
        },

        methods: {

            async submitGroupForm() {
                const token = localStorage.getItem("token");
                const response = await axios
                    .post("groups/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        name: this.name,
                        permissions: this.permissions,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully create a group..",
                        }).then((result) => {
                            this.$router.push("group-list");
                            console.log(result);
                        });
                        console.log(response);
                    })
                    .catch((error) => {
                        this.group_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
                console.log(response);
            },

            loadPermission() {
                const token = localStorage.getItem("token");
                axios
                    .get("permissions/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_permission = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            }
        },


        created() {
            this.loadPermission()
        },

        computed: {
            resultQuery() {
                if (this.searchQuery) {
                    return this.all_permission.filter((item) => {
                        return this.searchQuery.toLowerCase().split(' ').every(v => item.name.toLowerCase().includes(v))
                    })
                } else {
                    return this.all_permission;
                }
            }
        }
    }

</script>

<style scoped>
    .grp-per {
        height: 135px;
        /*overflow: scroll;*/
        overflow-y: scroll;
    }
</style>