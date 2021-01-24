<template>
  <div class="app-header header-shadow">
    <div class="app-header__logo">
      <router-link :to="{ name: 'Dashboard' }">
        <div class="logo-src"></div>
      </router-link>

      <div class="header__pane ml-auto">
        <div>
          <button
            type="button"
            class="hamburger close-sidebar-btn hamburger--elastic"
            data-class="closed-sidebar"
            :class="[is_active ? 'is-active' : null]"
            @click="collapseSideBar()"
          >
            <span class="hamburger-box">
              <span class="hamburger-inner"></span>
            </span>
          </button>
        </div>
      </div>
    </div>
    <div class="app-header__mobile-menu">
      <div>
        <button
          type="button"
          class="hamburger hamburger--elastic mobile-toggle-nav"
        >
          <span class="hamburger-box">
            <span class="hamburger-inner"></span>
          </span>
        </button>
      </div>
    </div>
    <div class="app-header__menu">
      <span>
        <button
          type="button"
          class="btn-icon btn-icon-only btn btn-primary btn-sm mobile-toggle-header-nav"
        >
          <span class="btn-icon-wrapper">
            <i class="fa fa-ellipsis-v fa-w-6"></i>
          </span>
        </button>
      </span>
    </div>

    <div class="app-header__content">
      <div class="app-header-left">
        <div class="search-wrapper">
          <div class="input-holder">
            <input
              type="text"
              class="search-input"
              placeholder="Type to search"
            />
            <button class="search-icon"><span></span></button>
          </div>
          <button class="close"></button>
        </div>
      </div>

      <div class="app-header-right">
        <div class="header-btn-lg pr-0">
          <div class="widget-content p-0">
            <div class="widget-content-wrapper">
              <div class="widget-content-left">
                <div class="widget-heading" v-if="first_name && last_name">
                  {{ first_name + " " + last_name }}
                </div>
                <div class="widget-heading" v-else>Your Name</div>

                <!-- <div class="widget-subheading">Admin</div> -->
              </div>

              <div class="widget-content-left ml-3 header-user-info">
                <div class="btn-group">
                  <a
                    data-toggle="dropdown"
                    aria-haspopup="true"
                    aria-expanded="false"
                    class="p-0 btn"
                  >
                    <!--                    <img v-if="user.image"-->
                    <!--                         :src="user.image"-->
                    <!--                         alt=""-->
                    <!--                         class="rounded-circle" height="40" width="40"-->
                    <!--                    />-->
                    <!--                    <img v-else-->
                    <!--                    <img v-else-->

                    <img
                      :src="image"
                      alt=""
                      class="rounded-circle"
                      height="40"
                      width="40"
                    />

                    <i class="fa fa-angle-down ml-2 opacity-8"></i>
                  </a>
                  <div
                    tabindex="-1"
                    role="menu"
                    aria-hidden="true"
                    class="dropdown-menu dropdown-menu-right"
                  >
                    <router-link
                      class="dropdown-item"
                      :to="{ name: 'UserProfile' }"
                    >
                      Profile
                    </router-link>

                    <div tabindex="-1" class="dropdown-divider"></div>

                    <button
                      type="button"
                      tabindex="0"
                      class="dropdown-item"
                      @click="logout"
                    >
                      Logout
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  props: {},
  data() {
    return {
      // user: null,
      is_active: false,
      first_name: null,
      last_name: null,
      image:
        "https://x1.xingassets.com/assets/frontend_minified/img/users/nobody_m.original.jpg",
    };
  },
  created() {
    const token = localStorage.getItem("token");

    if (token) {
      this.fetchAuthenticatedUser(token);
    }
  },
  methods: {
    collapseSideBar() {
      this.is_active = !this.is_active;
      this.$emit("collapse-sidebar", this.is_active);
    },
    fetchAuthenticatedUser() {
      const token = localStorage.getItem("token");

      axios
        .get("/profiles/me/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((response) => {
          this.first_name = response.data.user.first_name;
          this.last_name = response.data.user.last_name;
          this.image = response.data.image ? response.data.image : this.image;
          console.log(response.data);
        });
    },

    logout() {
      const token = localStorage.getItem("token");

      axios
        .get("logout/", {
          headers: {
            Authorization: `token ${token}`,
          },
        })
        .then((res) => {
          console.log(res.data);
        })
        .catch((error) => {
          console.error(error);
        });
      localStorage.removeItem("token");

      localStorage.clear();

      this.$router.push("/");
    },
    // end of Logout
  },
  // end of method
};
</script>
<style scoped>
#logoLink {
  text-decoration: none;
}
</style>
