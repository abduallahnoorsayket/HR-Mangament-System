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
                                Update User
                                <!-- <div class="page-title-subheading">Admin can edit user data from this form.
                                                            </div> -->
                            </div>
                        </div>
                    </div>
                </div>
                <div class="main-card card">
                    <div class="card-body">
                        <h5 class="card-title">Update User</h5>

                        <form @submit.prevent="editUser" v-if="form_data">
                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="exampleEmail11" class="">First Name</label>
                                            <input placeholder="first name" id="first_name"
                                                   v-model="form_data.first_name" type="text" class="form-control"/>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="exampleEmail11" class="">Last Name</label>
                                            <input placeholder="last name" id="last_name" v-model="form_data.last_name"
                                                   type="text" class="form-control"/>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="examplePassword11" class="">Password</label>
                                            <input name="password" id="password" v-model="form_data.password"
                                                   placeholder="password" type="password" class="form-control"/>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="exampleEmail11" class="">Email</label>
                                            <input name="email" id="username" v-model="form_data.username" type="email"
                                                   placeholder="Email" class="form-control"/>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="examplePassword11" class="">Phone</label>
                                            <input name="phone" id="phone" v-model="form_data.user_profile.phone"
                                                   placeholder="+880" type="text" class="form-control"/>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="examplePassword11" class="">Designation</label>
                                            <input
                                                    name="designation"
                                                    id="designation"
                                                    v-model="form_data.user_profile.designation"
                                                    placeholder="designation"
                                                    type="text"
                                                    class="form-control"
                                            />
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="examplePassword11" class="">Super User</label>
                                            <select name="select" id="superuser" v-model="form_data.is_superuser"
                                                    type="boolean" class="form-control">
                                                <option value="true">Yes</option>
                                                <option value="false">No</option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="col-md-6">
                                        <div class="position-relative form-group">
                                            <label for="examplePassword11" class="">Department</label>
                                            <select name="select" id="department"
                                                    v-model="form_data.user_profile.department" class="form-control">
                                                <option v-for="dprtmnt in all_departments" :key="dprtmnt.id"
                                                        :value="dprtmnt.id">
                                                    {{ dprtmnt.department_name }}
                                                </option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="col-md-6 group">
                                        <label>Group</label>
                                        <div class="col-md-6 grp-group" v-if="all_groups">
                                            <div class="position-relative form-check" v-for="grp in all_groups"
                                                 :key="grp.id">
                                                <label class="form-check-label">
                                                    <input type="checkbox" name="groups[]" :id="grp.id" :value="grp.id"
                                                           v-model="form_data.groups" class="form-check-input"/>
                                                    {{ grp.name }}
                                                </label>
                                            </div>
                                        </div>
                                    </div>

                                    <!-- {{groups}}-->
                                    <div class="vl mr-lg-5"></div>

                                    <div class="col-md-4 update-permission" style="margin-left: -40px">
                                        <label>Permissions</label><br/>

                                        <input type="text" v-model="searchQuery" placeholder="filter"
                                               style="outline: none; margin-bottom: 8px"/>

                                        <div class="grp-per" v-if="all_permission">
                                            <div class="position-relative form-check" v-for="prmsn in resultQuery"
                                                 :key="prmsn.id">
                                                <label class="form-check-label">
                                                    <input type="checkbox"
                                                           :id="prmsn.id"
                                                           :value="prmsn.id"
                                                           v-model="form_data.user_permissions"
                                                           class="form-check-input"
                                                    />
                                                    {{prmsn.name }}
                                                </label>
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <!-- {{user_permissions}}-->
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <div class="position-relative form-group mt-3">
                                            <label for="examplePassword11" class="">Branch</label>
                                            <select name="select" id="branch" v-model="form_data.user_profile.branch"
                                                    class="form-control">
                                                <option v-for="brnch in all_branchs" :key="brnch.id" :value="brnch.id">
                                                    {{ brnch.branch_name }}
                                                </option>
                                            </select>
                                        </div>
                                    </div>
                                    <div class="col-md-6">
                                        <div class="position-relative form-group mt-3">
                                            <label for="exampleAddress" class="">Location</label
                                            ><input
                                                name="address"
                                                id="location"
                                                v-model="form_data.user_profile.location"
                                                placeholder="Your Location"
                                                type="text"
                                                class="form-control"
                                        />
                                        </div>
                                    </div>
                                </div>
                                <div class="form-row">
                                    <div class="col-md-6">
                                        <label for="exampleAddress" class="">Education</label>
                                        <input
                                                name="address"
                                                id="education"
                                                v-model="form_data.user_profile.education"
                                                placeholder="Last Completed Degree"
                                                type="text"
                                                class="form-control"
                                        />
                                    </div>

                                    <div class="col-md-6">
                                        <label for="date" class="">Joining Date</label>
                                        <input
                                                name="address"
                                                id="date"
                                                v-model="form_data.user_profile.joining_date"
                                                placeholder="Your Skill"
                                                type="date"
                                                class="form-control"
                                        />
                                    </div>
                                </div>
                                <div class="position-relative form-group mt-3">
                                    <div>
                                        <label>Skill</label>
                                    </div>

                                    <input
                                            name="skill"
                                            id="skill"
                                            v-model="tagValue"
                                            v-on:keydown.enter.prevent="addTag"
                                            placeholder="skill"
                                            type="text"
                                            class="form-control"
                                    />
                                    <!--                                                    :class="{ 'is-invalid': form_data && form_data.user_profile.skills }"/>-->

                                    <div class="tag" v-for="(skill, index) in form_data.user_profile.skills"
                                         :key="'tag' + index">
                                        <span>{{ skill }}</span>
                                        <span @click="removeTag(index)"><i class="fas fa-times-circle"></i></span>
                                    </div>
                                </div>

                                <!-- {{user_permissions}}-->

                                <div class="position-relative form-group mt-5">
                                    <label for="exampleAddress2" class="">Short Bio</label>
                                    <textarea
                                            name="address2"
                                            v-model="form_data.user_profile.short_bio"
                                            placeholder="About Your Bio"
                                            type="text"
                                            class="form-control"
                                            rows="4"
                                            cols="50"
                                    ></textarea>

                                    <!-- <div class="position-relative form-group">
                                                                      <label for="img">Select image:</label>
                                                                      <input type="file" id="img" name="img" accept="image/*">
                                                                  </div> -->

                                    <div class="form-group">
                                        <button @submit.prevent="editUser"
                                                class="mt-2 btn btn-primary btn-lg float-right">Submit
                                        </button>
                                    </div>
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
    import Master from "../Master";
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "UpdateUser",
        components: {Master},
        data() {
            return {
                form_data: null,
                all_groups: null,
                all_permission: null,
                all_departments: null,
                all_branchs: null,

                searchQuery: null,

                tagValue: "",
            };
        },

        computed: {
            resultQuery() {
                if (this.searchQuery) {
                    return this.all_permission.filter((item) => {
                        return this.searchQuery
                            .toLowerCase()
                            .split(" ")
                            .every((v) => item.name.toLowerCase().includes(v));
                    });
                } else {
                    return this.all_permission;
                }
            },
        },

        methods: {
            loadGroup() {
                const token = localStorage.getItem("token");
                axios
                    .get("groups/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_groups = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
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
            },

            editUser() {
                const token = localStorage.getItem("token");
                axios
                    .put("users/" + this.$route.params.id + "/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                        // ...this.form_data
                        username: this.form_data.username,
                        // email: this.form_data.email,
                        password: this.form_data.password,
                        first_name: this.form_data.first_name,
                        last_name: this.form_data.last_name,
                        is_superuser: this.form_data.is_superuser,
                        groups: this.form_data.groups,
                        user_permissions: this.form_data.user_permissions,
                        user_profile: {
                            designation: this.form_data.user_profile.designation,
                            joining_date: this.form_data.user_profile.joining_date,
                            branch: this.form_data.user_profile.branch,
                            department: this.form_data.user_profile.department,
                            short_bio: this.form_data.user_profile.short_bio,
                            education: this.form_data.user_profile.education,
                            location: this.form_data.user_profile.location,
                            phone: this.form_data.user_profile.phone,
                            // skills: this.form_data.user_profile.skills ? this.form_data.user_profile.skills.split(',') : []
                            skills: this.form_data.user_profile.skills,
                        },
                    })
                    .then((response) => {
                        this.all_groups = response.data;
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            // title: "Yes...",
                            text: "You have successfully edit a user..",
                        }).then((response) => {
                            this.$router.push({name: "UserList"});
                            console.log(response);
                        });
                        console.log(response);
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            loadDepartment() {
                const token = localStorage.getItem("token");
                axios
                    .get("departments/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_departments = response.data;
                        this.all_departments = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },
            loadBranch() {
                const token = localStorage.getItem("token");
                axios
                    .get("branches/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_branchs = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            getUserData: function () {
                axios.get(`users/${this.$route.params.id}/`).then(
                    (response) => {
                        this.form_data = response.data;
                        // this.form_data.user_profile.skills = this.form_data.user_profile.skills
                        this.form_data.user_permissions = this.form_data.user_permissions.map((x) => x.id);
                        this.form_data.groups = this.form_data.groups.map((y) => y.id);
                        this.form_data.user_profile.department = this.form_data.user_profile.department.id;
                        this.form_data.user_profile.branch = this.form_data.user_profile.branch.id;
                    },
                    (response) => {
                        console.log(response);
                    }
                );
            },

            //Skills input tag function start
            addTag() {
                if (!this.tagValue == "") this.form_data.user_profile.skills.push(this.tagValue);

                this.tagValue = "";
            },

            removeTag(index) {
                this.form_data.user_profile.skills.splice(index, 1);
            },
            //Skills input tag function end
        },
        created() {
            this.loadGroup();
            this.loadPermission();
            this.loadDepartment();
            this.getUserData();
            this.loadBranch();
        },
    };
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

    .grp-group {
        height: 155px;
        /*overflow: scroll;*/
        overflow-y: scroll;
    }

    .tag {
        float: left;
        padding: 0px 5px;
        display: flex;
        justify-content: center;
        cursor: pointer;
        margin-bottom: -55px;
    }

    .tag span:first-child {
        margin-right: 8px;
        margin-bottom: 66px;
    }

    .tag svg {
        color: #666;
    }

    .tag svg :hover {
        color: #333;
    }
</style>
