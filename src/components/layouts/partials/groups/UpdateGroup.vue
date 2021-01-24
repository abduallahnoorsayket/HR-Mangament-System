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
                            <div> Update Groups
                                <!-- <div class="page-title-subheading">Admin can update group from this form.
                                </div> -->
                            </div>
                        </div>
                    </div>
                </div>

                <div class="main-card card">
                    <div class="card-body"><h5 class="card-title">Update Group</h5>

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <form @submit.prevent="EditGroup" v-if="form_data">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="exampleEmail11" class="">Name</label>
                                            <input placeholder="name" id="name" v-model="form_data.name"
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


                                    <div class="update-permission">
                                        <label>Permissions</label><br>
                                        <input type="text" v-model="searchQuery" placeholder="filter"
                                               style="outline: none;">
                                        <div class="grp-per" v-if="all_permission">

                                            <div class="position-relative form-check"
                                                 v-for="prmsn in resultQuery"
                                                 :key="prmsn.id">
                                                <label class="form-check-label">
                                                    <input
                                                            type="checkbox"
                                                            :id=prmsn.id
                                                            :value=prmsn.id
                                                            v-model="form_data.permissions"
                                                            class="form-check-input">{{ prmsn.name }}
                                                </label>
                                            </div>
                                        </div>
                                    </div>

                                </div>

                                <div class="form-group">
                                    <button @submit.prevent="EditGroup"
                                            class="mt-2 btn btn-success btn-lg float-right">Submit
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
        name: "UpdateGroup",
        components: {Master},
        data() {
            return {
                name: null,
                form_data: null,
                all_permission: null,

                searchQuery: null,

                group_error_data: {
                    name: null,
                }
            }
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
        },

        methods: {
            loadPermission() {
                const token = localStorage.getItem("token");
                axios.get('permissions/', {
                    headers: {
                        Authorization: `token ${token}`,
                    },
                })
                    .then(response => {
                        this.all_permission = response.data
                    })
                    .catch(function (error) {
                        console.log(error);
                    })
            },


            getGroupData: function () {
                axios.get(`groups/${this.$route.params.id}/`).then((response) => {
                    this.form_data = response.data
                    this.form_data.permissions = this.form_data.permissions.map(z => z.id)
                    console.log('----/////', this.form_data.permissions)
                }, (response) => {
                    console.log('----', response)
                });
            },

            EditGroup() {
                const token = localStorage.getItem("token");
                axios.put('groups/' + this.$route.params.id + '/', {
                    headers: {
                        Authorization: `token ${token}`,
                    },
                    name: this.form_data.name,
                    permissions: this.form_data.permissions
                })
                    .then(response => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully update a group..",
                        }).then(response => {
                            this.$router.push({name: 'GroupList'})
                            console.log(response)
                        })
                        console.log(response)
                    })
                    .catch((error) => {
                        this.group_error_data = error.response.data;
                        console.log("--++", error.response);
                    });
            },
        },


        created() {
            this.loadPermission()
            this.getGroupData()
        },

    }
</script>

<style scoped>


    .vl {
        border: 1px solid #eeeeee;
    }

    .grp-per {
        height: 135px;
        /*overflow: scroll;*/
        overflow-y: scroll;
    }

</style>