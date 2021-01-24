<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="fas fa-user-plus icon-gradient bg-tempting-azure"></i>
              </div>
              <div>
                Create New User
                <!-- <div class="page-title-subheading">Admin can create new user from this form.
                                                                            </div> -->
              </div>
            </div>
          </div>
        </div>

        <div class="main-card card">
          <div class="card-body">
            <!--                                <h5 class="card-title">Create User</h5>-->

            <!-- <form v-on:submit.prevent="submitUserForm">-->
            <form @submit.prevent="submitUserForm">
              <div class="form-group">
                <div class="form-row">
                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">First Name</label>
                      <input
                        placeholder="your first name"
                        id="first_name"
                        v-model="first_name"
                        type="text"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.first_name }"
                      />

                      <div :class="{ 'invalid-feedback': error_data && error_data.first_name }" v-if="error_data && error_data.first_name">
                        {{ error_data.first_name[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Last Name</label>
                      <input
                        placeholder="your last name"
                        id="last_name"
                        v-model="last_name"
                        type="text"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.last_name }"
                      />
                      <div :class="{ 'invalid-feedback': error_data && error_data.last_name }" v-if="error_data && error_data.last_name">
                        {{ error_data.last_name[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Email</label>
                      <input
                        placeholder="your email"
                        id="email"
                        v-model="username"
                        type="email"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.username }"
                      />
                      <div :class="{ 'invalid-feedback': error_data && error_data.username }" v-if="error_data && error_data.username">
                        {{ error_data.username[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Password</label>
                      <input
                        placeholder="password"
                        id="password"
                        v-model="password"
                        type="password"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.password }"
                      />
                      <div :class="{ 'invalid-feedback': error_data && error_data.password }" v-if="error_data && error_data.password">
                        {{ error_data.password[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label>Phone</label>
                      <input
                        placeholder="phone"
                        id="phone"
                        v-model="user_profile.phone"
                        type="text"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.user_profile.phone }"
                      />
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label for="exampleEmail11" class="">Designation</label>
                      <input
                        placeholder="designation"
                        id="designation"
                        v-model="user_profile.designation"
                        type="text"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.user_profile.designation }"
                      />

                      <div
                        :class="{ 'invalid-feedback': error_data && error_data.user_profile.designation }"
                        v-if="error_data && error_data.user_profile.designation"
                      >
                        {{ error_data.user_profile.designation[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label
                        for="example
                          11"
                        class="">Super User</label
                      >
                      <select
                        name="select"
                        id="superuser"
                        v-model="is_superuser"
                        type="boolean"
                        class="form-control"
                        :class="{
                          'is-invalid': error_data && error_data.is_superuser,
                        }"
                      >
                        <option value="true">Yes</option>
                        <option value="false">No</option>
                      </select>
                      <div
                        :class="{
                          'invalid-feedback': error_data && error_data.is_superuser,
                        }"
                        v-if="error_data && error_data.is_superuser"
                      >
                        {{ error_data.is_superuser[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group">
                      <label>Department</label>
                      <select
                        name="select"
                        id="department"
                        v-model="user_profile.department"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.user_profile.department }">

                        <option value="false" disabled selected>Select Department</option>
                        <option v-for="dprtmnt in all_departments" :key="dprtmnt.id" :value="dprtmnt.id">
                          {{ dprtmnt.department_name }}
                        </option>
                      </select>

                      <div
                        :class="{ 'invalid-feedback': error_data && error_data.user_profile.department }"
                        v-if="error_data && error_data.user_profile.department"
                      >
                        {{ error_data.user_profile.department[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6 group">
                    <label>Group</label>
                    <div class="col-md-6 grp-group" v-if="all_groups">
                      <div class="position-relative form-check" v-for="grp in all_groups" :key="grp.id">
                        <label class="form-check-label">
                          <input type="checkbox" name="groups[]" :id="grp.id" :value="grp.id" v-model="groups" class="form-check-input" />
                          {{ grp.name }}
                        </label>

                        <div :class="{ 'invalid-feedback': error_data && error_data.all_groups }" v-if="error_data && error_data.all_groups">
                          {{ error_data.all_groups[0] }}
                        </div>
                      </div>
                    </div>
                  </div>

                  <!-- {{groups}}-->
                  <div class="vl mr-lg-5"></div>

                  <div class="col-md-4 permissions" style="margin-left: -40px">
                    <label>Permissions</label> <br />

                    <input type="text" v-model="searchQuery" placeholder="filter"
                           style="outline: none; margin-bottom: 8px" />

                    <div class="grp-per" v-if="all_permission">
                      <div class="position-relative form-check" v-for="prmsn in resultQuery" :key="prmsn.id">
                        <label class="form-check-label">
                          <input
                            type="checkbox"
                            name="user_permissions"
                            :id="prmsn.id"
                            :value="prmsn.id"
                            v-model="user_permissions"
                            class="form-check-input"
                          />

                          {{ prmsn.name }}
                        </label>

                        <div :class="{ 'invalid-feedback': error_data && error_data.all_permission }" v-if="error_data && error_data.all_permission">
                          {{ error_data.all_permission[0] }}
                        </div>
                      </div>
                    </div>
                  </div>
                </div>

                <!-- {{user_permissions}}-->
                <div class="form-row">
                  <div class="col-md-6">
                    <div class="position-relative form-group mt-3">
                      <label>Branch</label>
                      <select
                        name="select"
                        id="department"
                        v-model="user_profile.branch"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.user_profile.branch }"
                      >
                        <option value="false" disabled selected>Select Branch</option>
                        <option v-for="brnch in all_branchs" :key="brnch.id" :value="brnch.id">
                          {{ brnch.branch_name }}
                        </option>
                      </select>

                      <div
                        :class="{ 'invalid-feedback': error_data && error_data.user_profile.branch }"
                        v-if="error_data && error_data.user_profile.branch"
                      >
                        {{ error_data.user_profile.branch[0] }}
                      </div>
                    </div>
                  </div>

                  <div class="col-md-6">
                    <div class="position-relative form-group mt-3">
                      <label for="exampleAddress" class="">Address</label
                      ><input
                        name="address"
                        id="location"
                        v-model="user_profile.location"
                        placeholder="Your Location"
                        type="text"
                        class="form-control"
                        :class="{ 'is-invalid': error_data && error_data.location }"
                      />

                      <div :class="{ 'invalid-feedback': error_data && error_data.location }" v-if="error_data && error_data.location">
                        {{ error_data.location[0] }}
                      </div>
                    </div>
                  </div>
                </div>
                <div class="form-row">
                  <div class="col-md-6">
                    <label for="exampleAddress" class="">Education</label>
                    <input
                      name="education"
                      id="education"
                      v-model="user_profile.education"
                      placeholder="Last Completed Degree"
                      type="text"
                      class="form-control"
                      :class="{ 'is-invalid': error_data && error_data.education }"
                    />

                    <div :class="{ 'invalid-feedback': error_data && error_data.education }" v-if="error_data && error_data.education">
                      {{ error_data.education[0] }}
                    </div>
                  </div>

                  <div class="col-md-6">
                    <label for="date" class="">Joining Date</label>
                    <input
                      name="joining-date"
                      id="date"
                      v-model="user_profile.joining_date"
                      placeholder=""
                      type="date"
                      class="form-control"
                      :class="{ 'is-invalid': error_data && error_data.user_profile.joining_date }"
                    />
                    <div
                      :class="{ 'invalid-feedback': error_data && error_data.user_profile.joining_date }"
                      v-if="error_data && error_data.user_profile.joining_date"
                    >
                      {{ error_data.user_profile.joining_date[0] }}
                    </div>
                  </div>
                </div>

                <div class="position-relative form-group mt-3">
                  <label>Skill</label>

                  <input
                    name="skill"
                    id="skill"
                    v-model="tagValue"
                    v-on:keydown.enter.prevent="addTag"
                    placeholder="skill"
                    type="text"
                    class="form-control"
                    :class="{ 'is-invalid': error_data && error_data.skills }"
                  />

                  <div class="tag" v-for="(skill, index) in user_profile.skills" :key="'tag' + index">
                    <span>{{ skill }}</span>
                    <span @click="removeTag(index)"><i class="fas fa-times-circle"></i></span>
                  </div>

                  <div :class="{ 'invalid-feedback': error_data && error_data.skills }" v-if="error_data && error_data.skills">
                    {{ error_data.skills[0] }}
                  </div>
                </div>

                <div class="position-relative form-group mt-5">
                  <div>
                    <label>Short Bio</label>
                  </div>
                  <textarea
                    name="short_bio"
                    id="short_bio"
                    v-model="user_profile.short_bio"
                    placeholder="About Your Bio"
                    type="text"
                    class="form-control"
                    rows="4"
                    cols="50"
                    :class="{ 'is-invalid': error_data && error_data.short_bio }"
                  ></textarea>

                  <div :class="{ 'invalid-feedback': error_data && error_data.short_bio }" v-if="error_data && error_data.short_bio">
                    {{ error_data.short_bio[0] }}
                  </div>
                </div>

                <div class="form-group">
                  <button @submit.prevent="submitUserForm" class="mt-2 btn btn-success btn-lg float-right">Submit</button>
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
  name: "CreateNewUser",
  components: { Master },
  data() {
    return {
      username: null,
      email: null,
      password: null,
      first_name: null,
      last_name: null,
      is_superuser: null,
      groups: [],
      user_permissions: [],
      user_profile: {
        designation: null,
        joining_date: null,
        department: null,
        branch: null,
        short_bio: null,
        education: null,
        location: null,
        phone: null,
        skills: [],
      },
      all_groups: null,
      all_permission: null,
      all_departments: null,
      all_branchs: null,

      tagValue: "",

      searchQuery: null,

      // error_data: null,
      error_data: {
        first_name: null,
        is_superuser: null,
        last_name: null,
        password: null,
        user_profile: {
          department: null,
          joining_date: null,
        },
        username: null,
      },
    };
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
          this.all_branchs = response.data;
        })
        .catch(function (error) {
          console.log(error);
        });
    },
    //Skills input tag function start
    addTag() {
      if (!this.tagValue == "") this.user_profile.skills.push(this.tagValue);

      this.tagValue = "";
    },
    removeTag(index) {
      this.user_profile.skills.splice(index, 1);
    },
    //Skills input tag function end

    submitUserForm() {
      const token = localStorage.getItem("token");
      axios
        .post("users/", {
          headers: {
            Authorization: `token ${token}`,
          },
          username: this.username,
          password: this.password,
          first_name: this.first_name,
          last_name: this.last_name,
          is_superuser: this.is_superuser,
          groups: this.groups,
          user_permissions: this.user_permissions,
          user_profile: {
            designation: this.user_profile.designation,
            joining_date: this.user_profile.joining_date,
            branch: this.user_profile.branch,
            department: this.user_profile.department,
            short_bio: this.user_profile.short_bio,
            education: this.user_profile.education,
            location: this.user_profile.location,
            phone: this.user_profile.phone,
            // skills: this.user_profile.skills ? this.user_profile.skills.split(",") : [],
            skills: this.user_profile.skills,
          },
        })
        .then((response) => {
          Swal.fire({
            icon: "success",
            // title: "Yes...",
            text: "You have successfully create a user..",
          }).then((result) => {
            this.$router.push("user-list");
            console.log(result);
          });
          console.log(response);
        })
        .catch((error) => {
          this.error_data = error.response.data;
          console.log("--++", error.response);
        });
    },
  },
  created() {
    this.loadGroup();
    this.loadPermission();
    this.loadDepartment();
    this.loadBranch();
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
