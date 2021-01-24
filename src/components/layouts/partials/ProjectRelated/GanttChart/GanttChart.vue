<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner" id="recordsModal">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>Project Details</div>
                        </div>
                    </div>
                </div>
                <nav class="navbar navbar-expand-lg navbar navbar navbar-light" style="background-color: #e3f2fd;">
                    <div class="collapse navbar-collapse" id="navbarNav">
                        <ul class="navbar-nav">
                            <li class="nav-item ">
                                <router-link class="nav-link" :to="{name: 'ProjectDetails'}">
                                    Project Overview
                                </router-link>
                            </li>
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'IssueList'}">
                                    Issues
                                </router-link>
                            </li>
                            <li class="nav-item active">
                                <router-link class="nav-link" :to="{name: 'AddIssue'}">
                                    Add New Issue
                                </router-link>
                            </li>
                            <li class="nav-item">
                                <a class="nav-link " href="#">Activity</a>
                            </li>
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'GanttChart'}">
                                    <strong>Gantt Chart</strong>
                                </router-link>
                            </li>
                            <li class="nav-item">
                                <router-link class="nav-link" :to="{name: 'FilesList'}">
                                    Files
                                </router-link>
                            </li>
                        </ul>
                    </div>
                </nav>

                <!--Issue Form-->
                <button>gantt chart</button>

            </div>
        </template>
    </Master>
</template>

<script>
    import Master from "../../../Master";

    export default {
        name: "GanttChart",
        components: {Master},
        data() {
            return {};
        },

        methods: {}
    };
</script>

<style scoped>
    .nav-item {
        margin: 0px 30px;
    }
</style>