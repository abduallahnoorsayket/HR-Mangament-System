<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="container emp-profile" v-if="user">
                    <div class="mb-0 card card-body">
                        <div class="row">
                            <div class="col-md-12">
                                <div class="float-left">
                                    <h1 class="card-header">My Profile</h1>
                                </div>
                                <div class="float-right">
                                    <div class="header">
                                        <router-link :to="{ name: 'UpdateProfile', params: { id: user.id }   }">
                                            <button type="button" class="btn btn-primary btn-sm">Edit Profile</button>

                                        </router-link>

                                    </div>

                                </div>
                                <div class="float-right">
                                    <div class="header">
                                        <router-link :to="{ name: 'UpdatePassword' }">
                                            <button type="button" class="btn btn-primary btn-sm">Change Password
                                            </button>
                                        </router-link>
                                    </div>
                                </div>
                            </div>

                        </div>
                        <div class="profileColor">
                            <div class="row">
                                <div class="col-md-4">
                                    <div class="profile-img">
                                        <img v-if="user.image"
                                             :src="user.image"
                                             alt=""
                                             class="rounded-circle"
                                             height="20" width="20"
                                        />
                                        <img
                                                v-else
                                                width="20"
                                                height="20"
                                                class="rounded-circle"
                                                src="https://x1.xingassets.com/assets/frontend_minified/img/users/nobody_m.original.jpg"
                                                alt=""
                                                @error="
                  $event.target.src = '../assets/images/avatars/default.png'
                "
                                        />
                                    </div>
                                </div>
                                <div class="col-md-8">
                                    <div class="profile-head">
                                        <h5 v-if="user.user.first_name && user.user.last_name">
                                            {{ user.user.first_name + " " + user.user.last_name }}
                                        </h5>
                                        <p v-if="user"> {{user.designation}}</p>


                                        <div class="contactInfo">
                                            <div class="row">
                                                <div class="col-md-3">
                                                    <label>Employee ID</label>
                                                </div>

                                                <div class="col-md-6">
                                                    <h6 v-if="user">
                                                        {{ user.employee_id }}
                                                    </h6>


                                                </div>
                                            </div>
                                            <div class="row">
                                                <div class="col-md-3">
                                                    <label>Joining Date</label>
                                                </div>

                                                <div class="col-md-6">
                                                    <h6 v-if="user.joining_date">
                                                        {{ user.joining_date }}
                                                    </h6>
                                                </div>
                                            </div>
                                            <div class="row">
                                                <div class="col-md-3">
                                                    <label>Department</label>
                                                </div>

                                                <div class="col-md-6">
                                                    <h6 v-if="user">
                                                        {{ user.department.department_name }}
                                                    </h6>
                                                </div>
                                            </div>
                                            <div class="row">
                                                <div class="col-md-3">
                                                    <label>Branch</label>
                                                </div>

                                                <div class="col-md-6">
                                                    <h6 v-if="user">
                                                        {{ user.branch.branch_name }}
                                                    </h6>
                                                </div>
                                            </div>

                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <hr/>
                        <div class="row">
                            <div class="col-md-6">
                                <div class="employee">
                                    <h4><strong>Personal Info</strong></h4>
                                    <br/>

                                    <div class="row">
                                        <div class="col-md-6">
                                            <label>Email</label>
                                        </div>
                                        <div class="col-md-6">
                                            <p v-if="user.user.username">
                                            <ul id="menu">
                                                <li>
                                                    {{ user.user.username }}
                                                </li>
                                            </ul>

                                            </p>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <label>Education</label>
                                        </div>
                                        <div class="col-md-6">
                                            <p v-if="user.education">
                                            <ul id="menu">
                                                <li>
                                                    {{ user.education }}
                                                </li>
                                            </ul>

                                            </p>
                                        </div>
                                    </div>

                                    <div class="row">
                                        <div class="col-md-6">
                                            <label>Location</label>
                                        </div>
                                        <div class="col-md-6">
                                            <p v-if="user.education">
                                            <ul id="menu">
                                                <li>
                                                    {{ user.location }}
                                                </li>
                                            </ul>

                                            </p>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <label>Skills</label>
                                        </div>
                                        <div class="col-md-6">
                                            <ul id="menu" v-if="user.skills">
                                                <li v-for="skill in user.skills" :key="skill">
                                                    {{ skill + " " }}
                                                </li>
                                            </ul>
                                        </div>
                                    </div>
                                    <div class="row">
                                        <div class="col-md-6">
                                            <label>Phone</label>
                                        </div>
                                        <div class="col-md-6">
                                            <p v-if="user.phone">
                                            <ul id="menu">
                                                <li>
                                                    {{ user.phone }}
                                                </li>
                                            </ul>

                                            </p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-6">
                                <div class="employeeRole">
                                    <h4><strong></strong></h4>
                                    <br>

                                    <div class="row mt-4">
                                        <div class="col-md-6">
                                            <label>User Role</label>
                                        </div>
                                        <div class="col-md-6">
                                            <ul id="menu" v-for="group in user.user.groups" :key="group">
                                                <li>
                                                    {{group.name}}
                                                </li>
                                            </ul>
                                            <p>


                                            </p>
                                        </div>
                                    </div>

                                </div>
                            </div>

                        </div>

                        <hr/>
                        <div class="col-md-12">
                            <div class="employee">
                                <h4><strong>Short Bio</strong></h4>
                                <br/>
                                <p class="" v-if="user.short_bio">
                                    {{ user.short_bio }}
                                </p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </template>
    </Master>

</template>


<script>
    import axios from "axios";
    import Master from "../Master";

    export default {
        name: "UserProfile",
        components: {Master},
        data() {
            return {
                user: null,
            };
        },

        created() {
            const token = localStorage.getItem("token");

            if (token) {
                this.fetchAuthenticatedUser(token);
            }
        },
        //   end of creted
        methods: {
            fetchAuthenticatedUser() {
                axios.get("/profiles/me/").then((response) => {
                    this.user = response.data;
                    console.log(response.data);
                });
            },
        },
    };
</script>

<style scoped>

    /* new profile page */
    input.hidden {
        position: absolute;
        left: -9999px;
    }

    #profile-image1 {
        cursor: pointer;

        width: 100px;
        height: 100px;
        border: 2px solid #03b1ce;
    }

    .tital {
        font-size: 16px;
        font-weight: 500;
    }

    .bot-border {
        border-bottom: 1px #f8f8f8 solid;
        margin: 5px 0 5px 0;
    }

    /*  for profile page */
    body {
        background: -webkit-linear-gradient(left, #3931af, #00c6ff);
    }

    .tab-pane fade show active {
        margin-left: 20px;
    }

    .employee {
        margin: 20px 60px;
    }

    .employeeRole {
        margin: 28px 60px;
    }

    .employee_bio {
        margin-bottom: 20px;
    }

    .header {
        margin-bottom: 10px;
        margin-top: 10px;
        margin-left: 20px;
        margin-right: 20px;
    }

    .bio {
        margin-bottom: 20px;
    }

    ul#menu li {
        display: inline;
        list-style: none;

        /* list-style: square; */
    }

    .contactInfo {
        margin-top: 28px;
        margin-bottom: 10px;
    }

    .profileColor {
        background-color: #f2f2f2;
        padding: 20px 0px;
        margin-bottom: 20px;
    }

    .card-body {
        padding: 10px 0px;
    }

    .emp-profile {
        padding: 0;
        margin-top: 3%;
        margin-bottom: 3%;
        /* border-radius: 0rem; */
        /* background: #fff; */
        padding-bottom: 10px;
        padding-top: 10px;
        /* padding-right: ; */
    }

    .profile-img {
        text-align: center;
    }

    .profile-img img {
        width: 70%;
        height: 100%;
    }

    .profile-img .file {
        position: relative;
        overflow: hidden;
        margin-top: -20%;
        width: 70%;
        border: none;
        border-radius: 0;
        font-size: 15px;
        background: #212529b8;
    }

    .profile-img .file input {
        position: absolute;
        opacity: 0;
        right: 0;
        top: 0;
    }

    .profile-head {
        margin-top: 20px;
    }

    .profile-head h5 {
        color: #333;
    }

    .profile-head h6 {
        color: #0062cc;
    }

    btns btns-primary {
        padding: 2%;
        width: 80%;
        cursor: pointer;
        font-weight: 200;
    }

    .profile-edit-btn {
        border: none;
        border-radius: 1rem;
        width: 85%;
        padding: 2% 8% 2% 5%;
        /* padding-left:;
        padding-right: ; */

        font-weight: 400;
        color: #6c757d;
        cursor: pointer;
        margin-top: 10px;
        /* margin-right: 5px; */
    }

    .proile-rating {
        font-size: 12px;
        color: #818182;
        margin-top: 5%;
    }

    .proile-rating span {
        color: #495057;
        font-size: 15px;
        font-weight: 600;
    }

    .profile-head .nav-tabs {
        margin-bottom: 5%;
    }

    .profile-head .nav-tabs .nav-link {
        font-weight: 600;
        border: none;
    }

    .profile-head .nav-tabs .nav-link.active {
        border: none;
        border-bottom: 2px solid #0062cc;
    }

    .profile-work {
        padding: 14%;
        /* margin-top:0; */
        margin-left: 5px;
    }

    .profile-work p {
        font-size: 12px;
        color: #818182;
        font-weight: 600;
        margin-top: 10%;
    }

    .profile-work a {
        text-decoration: none;
        color: #495057;
        font-weight: 600;
        font-size: 14px;
    }

    /* .profile-work ul {
      list-style: none;
    } */
    .profile-tab label {
        font-weight: 600;
    }

    .profile-tab p {
        font-weight: 600;
        color: #0062cc;
    }

    hr {
        margin-top: 25px;
        margin-bottom: 1rem;
        border: 0;
        border-top: 1px solid rgba(0, 0, 0, 0.1);
    }
</style>