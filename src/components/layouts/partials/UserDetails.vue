<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="container emp-profile" v-if="user">
          <div class="mb-0 card card-body">
            <div class="row">
              <div class="col-md-8">
                <h4 class="card-header">User Details</h4>
              </div>
              <div class="col-md-2"></div>
              <div class="col-md-2">
                <router-link :to="{ name: 'UpdateUser', params: { id: user.id } }">
                  <button type="button" class="btns btn-primary btn-sm mt-2">Edit User</button>
                  <!-- <input
                    type="submit"
                    class="profile-edit-btn"
                    name="btnAddMore"
                    value="Edit Profile"
                  /> -->
                </router-link>
              </div>
            </div>
            <div class="profileColor">
              <div class="row">
                <div class="col-md-4">
                  <div class="profile-img">
                    <img v-if="user.user_profile.image" :src="user.user_profile.image" alt="" class="rounded-circle" />
                    <img
                      v-else
                      src="https://x1.xingassets.com/assets/frontend_minified/img/users/nobody_m.original.jpg"
                      alt=""
                      class="rounded-circle"
                    />
                  </div>
                </div>
                <div class="col-md-8">
                  <div class="profile-head">
                    <h5 v-if="user.first_name && user.last_name">
                      {{ user.first_name + " " + user.last_name }}
                    </h5>
                    <p v-if="user">{{ user.user_profile.designation }}</p>
                    <div class="contactInfo">
                      <div class="row">
                        <div class="col-md-2">
                          <label>Email</label>
                        </div>
                        <div class="col-md-8">
                          <h6 v-if="user.username">
                            {{ user.username }}
                          </h6>
                        </div>
                      </div>
                      <div class="row">
                        <div class="col-md-2">
                          <label>Phone</label>
                        </div>
                        <div class="col-md-8">
                          <h6 v-if="user.user_profile.phone">
                            {{ user.user_profile.phone }}
                          </h6>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <hr />
            <div class="row">
              <div class="col-md-6">
                <div class="employee">
                  <h4><strong>Employee Info</strong></h4>
                  <br />
                  <div class="row">
                    <div class="col-md-6">
                      <h6>Employee ID</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.employee_id">
                        <h6>
                          {{ user.user_profile.employee_id }}
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <h6>Joining Date</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.joining_date">
                        <h6>
                          {{ user.user_profile.joining_date }}
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <h6>Branch Name</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.branch.branch_name">
                        <h6>
                          {{ user.user_profile.branch.branch_name }}
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <h6>Department Name</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.department.department_name">
                        <h6>
                          {{ user.user_profile.department.department_name }}
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <h6>Education</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.education">
                        <h6>
                          {{ user.user_profile.education }}
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <label>Location</label>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.location">
                        <h6>
                          {{ user.user_profile.location }}
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <h6>Skills</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_profile.skills">
                        <h6 v-for="skill in user.user_profile.skills" :key="skill">
                          {{ skill + ",  " }}
                        </h6>
                      </h6>
                    </div>
                  </div>
                </div>
              </div>
              <div class="col-md-6">
                <div class="employee">
                  <h4><strong>Groups & Permissions</strong></h4>
                  <br />
                  <div class="row">
                    <div class="col-md-6">
                      <h6>User Groups</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.groups">
                        <h6 v-for="group in user.groups" :key="group">
                          <p>{{ group.name }}</p>
                        </h6>
                      </h6>
                    </div>
                  </div>

                  <div class="row">
                    <div class="col-md-6">
                      <h6>User Permissions</h6>
                    </div>
                    <div class="col-md-6">
                      <h6 id="menu" v-if="user.user_permissions">
                        <h6 v-for="permission in user.user_permissions" :key="permission">
                          <p>{{ permission.name }}</p>
                          <p>
                            {{ permission.content_type.app_label }}
                          </p>
                        </h6>
                      </h6>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <hr />
            <div class="col-md-12">
              <div class="employee">
                <h4><strong>Short Bio</strong></h4>
                <br />
                <h6 class="" v-if="user.user_profile.short_bio">
                  {{ user.user_profile.short_bio }}
                </h6>
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
  name: "UserDetails",
  components: { Master },
  data() {
    return {
      user: null,
    };
  },

  created() {
    axios.get(`users/${this.$route.params.id}/`, {}).then((response) => {
      this.user = response.data;
      console.log(this.user);
    });
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
  line-height: 100%;
}
.employee h6 {
  line-height: 180%;
}

.employee_bio {
  margin-bottom: 20px;
}

.header {
  margin-bottom: 10px;
  margin-top: 10px;
  margin-left: 20px;
}

.bio {
  margin-bottom: 20px;
}

h6#menu h6 {
  display: inline;
  /* list-style: none; */
  list-style: square;
  margin-left: 0;
}
.contactInfo {
  margin-top: 42px;
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
  margin-top: 30px;
}

.profile-head h5 {
  color: #333;
}

.profile-head h6 {
  color: #0062cc;
}
.btns {
  padding: 2%;
  width: 85%;
}

.profile-edit-btn {
  border: none;
  border-radius: 1.5rem;
  width: 70%;
  padding: 2%;
  font-weight: 600;
  color: #6c757d;
  cursor: pointer;
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
