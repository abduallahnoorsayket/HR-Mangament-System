<template>
    <div class="app-sidebar sidebar-shadow">
        <div class="app-header__logo">
            <div class="logo-src"></div>
            <div class="header__pane ml-auto">
                <div>
                    <button
                            type="button"
                            class="hamburger close-sidebar-btn hamburger--elastic"
                            data-class="closed-sidebar"
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


        <!--Sidebar Menu Start-->
        <div class="scrollbar-sidebar">
            <div class="app-sidebar__inner">
                <!--Authentication-->
                <ul class="vertical-nav-menu">
                    <li v-if="hasModulePermission('auth', 'hrm_auth')"
                        class="app-sidebar__heading">
                        Authentication <br>
                        <!--ß{{currentRoutePath ('/create-new-user')}}-->
                    </li>
                    <!--User-->
                    <li v-if="hasModelPermission('user')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-users"></i>
                            Users
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>

                        </a>
                        <ul>
                            <li v-if="hasPermission('add_user')">
                                <router-link :to="{ path: '/create-new-user' }"
                                             :class="{'mm-active' : currentRoutePath('/create-new-user')}">
                                    Create New User
                                </router-link>
                            </li>
                            <li v-if="hasPermission('view_user')">
                                <router-link :to="{ path: '/user-list' }"
                                             :class="{'mm-active' : currentRoutePath('/user-list')}">
                                    User List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Groups-->
                    <li v-if="hasModelPermission('group')">
                        <a href="#">
                            <i class="metismenu-icon fas pe-7s-id"></i>
                            Groups
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <!--add_group-->
                            <li v-if="hasPermission('add_group')">
                                <router-link :to="{ path: '/create-group' }"
                                             :class="{'mm-active' : currentRoutePath('/create-group')}">
                                    Create Group
                                </router-link>
                            </li>

                            <!--view_group-->
                            <li v-if="hasPermission('view_group')">
                                <router-link :to="{ path: '/group-list' }"
                                             :class="{'mm-active' : currentRoutePath('/group-list')}">
                                    Group List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Department-->
                    <li v-if="hasModelPermission('department')">
                        <a href="#">
                            <i class="metismenu-icon fas pe-7s-photo-gallery"></i>
                            Departments
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_department')">
                                <router-link :to="{ path: '/create-department' }"
                                             :class="{'mm-active' : currentRoutePath('/create-department')}">
                                    Create Department
                                </router-link>
                            </li>
                            <li v-if="hasPermission('view_department')">
                                <router-link :to="{ path: '/department-list' }"
                                             :class="{'mm-active' : currentRoutePath('/department-list')}">
                                    Department List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Branch-->
                    <li v-if="hasModelPermission('branch')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-network"></i>
                            Branch
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_branch')">
                                <router-link :to="{ path: '/create-branch' }"
                                             :class="{'mm-active' : currentRoutePath('/create-branch')}">
                                    Create Branch
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_branch')">
                                <router-link :to="{ path: '/branch-list' }"
                                             :class="{'mm-active' : currentRoutePath('/branch-list')}">
                                    Branch List
                                </router-link>
                            </li>
                        </ul>
                    </li>
                </ul>
                <!--Authentication End-->


                <!--Attendance Start-->
                <ul class="vertical-nav-menu">
                    <li v-if="hasModulePermission('hrm_attendance')"
                        class="app-sidebar__heading">
                        Attendance <br>
                    </li>

                    <!--Attendance Record-->
                    <li v-if="hasModelPermission('attendancerecord')">
                        <router-link :to="{ path: '/attendance-records' }"
                                     :class="{'mm-active' : currentRoutePath('/attendance-records')}">
                            <i class="metismenu-icon pe-7s-news-paper"></i>
                            Attendance Record
                        </router-link>
                    </li>

                    <!--Holiday-->
                    <li v-if="hasModelPermission('holiday')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-coffee"></i>
                            Holiday
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_holiday')">
                                <router-link :to="{ path: '/add-holiday' }"
                                             :class="{'mm-active' : currentRoutePath('/add-holiday')}">
                                    Create Holiday
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_holiday')">
                                <router-link :to="{ path: '/holiday-list' }"
                                             :class="{'mm-active' : currentRoutePath('/holiday-list')}">
                                    Holiday List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Branch Holiday-->
                    <li v-if="hasModelPermission('branchholiday')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-map-2"></i>
                            Branch Holiday
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_branchholiday')">
                                <router-link :to="{ path: '/create-holiday' }"
                                             :class="{'mm-active' : currentRoutePath('/create-holiday')}">
                                    Create Branch Holiday
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_branchholiday')">
                                <router-link :to="{ path: '/branch-holiday' }"
                                             :class="{'mm-active' : currentRoutePath('/branch-holiday')}">
                                    Branch Holiday List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Leave Type-->
                    <li v-if="hasModelPermission('leavetype')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-bandaid"></i>
                            Leave Type
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_leavetype')">
                                <router-link :to="{ path: '/create-leave-types' }"
                                             :class="{'mm-active' : currentRoutePath('/create-leave-types')}">
                                    Create Leave Type
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_leavetype')">
                                <router-link :to="{ path: '/leave-types' }"
                                             :class="{'mm-active' : currentRoutePath('/leave-types')}">
                                    Leave Type List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--All Leave-->
                    <li v-if="hasModelPermission('leave')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-paperclip"></i>
                            All Leaves
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_leave')">
                                <router-link :to="{ path: '/add-leaves-request' }"
                                             :class="{'mm-active' : currentRoutePath('/add-leaves-request')}">
                                    Create Leave Request
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_leave')">
                                <router-link :to="{ path: '/all-leaves' }"
                                             :class="{'mm-active' : currentRoutePath('/all-leaves')}">
                                    All Leaves List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--My Leave History-->
                    <li v-if="hasPermission('view_leavetype')">
                        <router-link :to="{ path: '/my-leave-history' }"
                                     :class="{'mm-active' : currentRoutePath('/my-leave-history')}">
                            <i class="metismenu-icon pe-7s-back-2"></i>
                            My Leave History
                        </router-link>
                    </li>
                </ul>
                <!--Attendance End-->


                <!--Evaluation Start-->
                <ul class="vertical-nav-menu">
                    <li v-if="hasModulePermission('hrm_evaluation')"
                        class="app-sidebar__heading">
                        Evaluation <br>
                    </li>

                    <!--Rating-->
                    <li v-if="hasModelPermission('rating')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-star"></i>

                            Rating
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_rating')">
                                <router-link :to="{ path: '/create-rating' }"
                                             :class="{'mm-active' : currentRoutePath('/create-rating')}">
                                    Create Rating
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_rating')">
                                <router-link :to="{ path: '/rating-list' }"
                                             :class="{'mm-active' : currentRoutePath('/rating-list')}">
                                    Rating List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Rating Point-->
                    <li v-if="hasModelPermission('ratingpoint')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-graph2"></i>
                            Rating Point
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_ratingpoint')">
                                <router-link :to="{ path: '/create-rating-point' }"
                                             :class="{'mm-active' : currentRoutePath('/create-rating-point')}">
                                    Create Rating Point
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_ratingpoint')">
                                <router-link :to="{ path: '/rating-point-list' }"
                                             :class="{'mm-active' : currentRoutePath('/rating-point-list')}">
                                    Rating Point List
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--Add Evaluation-->
                    <li v-if="hasModelPermission('employeeinformation')">
                        <router-link :to="{ path: '/add-evaluation' }"
                                     :class="{'mm-active' : currentRoutePath('/add-evaluation')}">
                            <i class="metismenu-icon pe-7s-note2"></i>
                            Add Evaluation
                        </router-link>
                    </li>

                    <!--Evaluation Graph-->
                    <li v-if="hasModelPermission('employeeinformation')">
                        <a href="#">
                            <i class="metismenu-icon pe-7s-graph1"></i>
                            Evaluation Graph
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('view_employeeinformation')">
                                <router-link :to="{ path: '/single-member' }"
                                             :class="{'mm-active' : currentRoutePath('/single-member')}">
                                    Single Member
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_employeeinformation')">
                                <router-link :to="{ path: '/department-wise' }"
                                             :class="{'mm-active' : currentRoutePath('/department-wise')}">
                                    Department Wise
                                </router-link>
                            </li>

                            <li v-if="hasPermission('view_employeeinformation')">
                                <router-link :to="{ path: '/my-evaluation' }"
                                             :class="{'mm-active' : currentRoutePath('/my-evaluation')}">
                                    My Evaluation
                                </router-link>
                            </li>
                        </ul>
                    </li>

                </ul>
                <!--Evaluation End-->

                 <!--Project  Start-->
                <ul class="vertical-nav-menu">
                    <li v-if="hasModulePermission('hrm_evaluation')"
                        class="app-sidebar__heading">
                        Project Related <br>
                    </li>
                    <!--Client -->
                    <li v-if="hasModelPermission('employeeinformation')">
                          <a href="#">
                            <i class="metismenu-icon pe-7s-user"></i>
                            Client
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_ratingpoint')">
                                <router-link :to="{ path: '/add-client' }"
                                             :class="{'mm-active' : currentRoutePath('/add-client')}">
                                    Add Client
                                </router-link>
                            </li>
                            <li v-if="hasPermission('view_ratingpoint')">
                                <router-link :to="{ path: '/client-list' }"
                                             :class="{'mm-active' : currentRoutePath('/client-list')}">
                                    Client list
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!-- Project-->
                    <li v-if="hasModelPermission('employeeinformation')">
                          <a href="#">
                            <i class="metismenu-icon pe-7s-drawer"></i>
                            Projects
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <li v-if="hasPermission('add_ratingpoint')">
                                <router-link :to="{ path: '/add-new-project' }"
                                             :class="{'mm-active' : currentRoutePath('/add-new-project')}">
                                    Add New Project
                                </router-link>
                            </li>
                            <li v-if="hasPermission('view_ratingpoint')">
                                <router-link :to="{ path: '/project-list' }"
                                             :class="{'mm-active' : currentRoutePath('/project-list')}">
                                    Project list
                                </router-link>
                            </li>
                        </ul>
                    </li>

                    <!--All Category-->
                    <li v-if="hasModelPermission('employeeinformation')">
                          <a href="#">
                            <i class="metismenu-icon pe-7s-drawer"></i>
                            All Category
                            <i class="metismenu-state-icon pe-7s-angle-down caret-left"></i>
                        </a>
                        <ul>
                            <!--Work Type-->
                            <li v-if="hasPermission('view_ratingpoint')">
                                <router-link :to="{ path: '/work-type' }"
                                             :class="{'mm-active' : currentRoutePath('/work-type')}">
                                    Work Type
                                </router-link>
                            </li>

                            <!--Work Status-->
                            <li v-if="hasPermission('view_ratingpoint')">
                                <router-link :to="{ path: '/work-status' }"
                                             :class="{'mm-active' : currentRoutePath('/work-status')}">
                                    Work Status
                                </router-link>
                            </li>

                            <!--Work Priority-->
                            <li v-if="hasPermission('view_ratingpoint')">
                                <router-link :to="{ path: '/work-priority' }"
                                             :class="{'mm-active' : currentRoutePath('/work-priority')}">
                                    Work Priority
                                </router-link>
                            </li>
                        </ul>
                    </li>
                </ul>
                <!--Project End-->

            </div>
        </div>
        <!--Sidebar Menu End-->

    </div>
</template>

<script>
    import permissions from "@/authorization/permissions";

    export default {
        name: "Sidebar",
        data() {
            return {}
        },// data

        methods: {

            currentRoutePath(routeName) {
                return this.$route.path === routeName
            },
            hasModulePermission(...module_name) {
                return permissions.hasModulePermission(...module_name)
                // return permissions.hasModulePermission.apply(...module_name)
            },

            hasModelPermission(model_name) {
                return permissions.hasModelPermission(model_name)
            },

            hasPermission(permission_name) {
                return permissions.hasPermission(permission_name)
            },

        },// methods

        created() {

        },// created

    };// export default

</script>

<style scoped>
    /* @import ".../main.css"; */
</style>

