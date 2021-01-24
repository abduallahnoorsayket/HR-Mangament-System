<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-eye icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div> Groups Details
                                <!-- <div class="page-title-subheading">Admin can create new user from this form.
                                </div> -->
                            </div>

                            <router-link :to="{path: '/group-list'}">
                                <a class="float" style="position: absolute; right: 0">
                                    <i class="fas fa-arrow-alt-circle-left"
                                       style="color: #72e3da; font-size: 33px"></i>
                                </a>
                            </router-link>

                        </div>
                    </div>
                </div>

                <div class="main-card card" v-if="group">
                    <div class="card-body">

                        <!-- <form v-on:submit.prevent="submitUserForm">-->
                        <div class="form-group">
                            <div class="form-row">
                                <div class="col-md-6">
                                    <div class="position-relative form-group">
                                        <div class="position-relative form-check" style="margin-left: -20px"
                                             v-if="group.name">
                                            <b>{{'Group Name: ' + group.name }}</b>
                                        </div>
                                    </div>
                                </div>
                            </div>

                            <div class="permissions">
                                <label>Permissions</label>
                                <div class="position-relative form-check"
                                     v-for="prmsn in group.permissions"
                                     :key="prmsn.id">
                                    <li> {{ prmsn.name }}</li>
                                </div>
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

    export default {
        name: "GroupDetails",
        components: {Master},
        data() {
            return {
                group: null,
            }
        },


        created() {
            axios.get(`groups/${this.$route.params.id}/`, {}).then((response) => {
                this.group = response.data;
                console.log('####', this.group);
            });
        },
    }
</script>


<style scoped>

</style>