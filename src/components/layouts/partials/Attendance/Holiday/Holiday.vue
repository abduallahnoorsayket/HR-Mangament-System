<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <!-- <Modal /> -->
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Branch Holidays
                                <!-- <div class="page-title-subheading">Admin can show the group list from here.

                                                                            </div> -->
                            </div>
                            <router-link :to="{ path: '/create-holiday' }">
                                <a class="float" style="position: absolute; right: 0">
                                    <button type="button" class="btn btn-success" style="font-size: 15px"><span
                                            class="fas fa-plus-square"></span> Add Branch Holiday
                                    </button>
                                </a>
                            </router-link>
                        </div>
                    </div>
                </div>
                <!--            </div>-->

                <!--            </div>-->
                <div id="accordion" class="acordinContainer">
                    <div class="card">
                        <div id="headingOne" class="">
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="headerContaier">
                                        <div class="float-left"><h5>Advanced Search</h5></div>

                                        <div class="float-right">
                                            <i
                                                    id="addIcon"
                                                    class="fa fa-chevron-circle-down"
                                                    style="font: 45px"
                                                    data-toggle="collapse"
                                                    data-target="#collapseOne1"
                                                    aria-expanded="true"
                                                    aria-controls="collapseOne"
                                            ></i>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <div data-parent="#accordion" id="collapseOne1" aria-labelledby="headingOne" class="collapse">
                            <div class="card-body">
                                <form action="" @submit.prevent="searchHoliday">
                                    <div class="form-row">
                                        <div class="col-md-4">
                                            <label>Branch </label>
                                            <div class="form-group">
                                                <select name="select" id="branch" v-model="branch" class="form-control">
                                                    <option value="" selected>Select Branch</option>
                                                    <option v-for="brnch in all_branchs" :key="brnch.id"
                                                            :value="brnch.id">
                                                        {{ brnch.branch_name }}
                                                    </option>
                                                </select>
                                            </div>
                                        </div>

                                        <div class="col-md-4">
                                            <label>Purpose </label>
                                            <input type="text" v-model="purpose" class="form-control"/>
                                        </div>

                                        <div class="col-md-4">
                                            <label> Type</label>

                                            <select name="select" id="type" v-model="type" type="choice"
                                                    class="form-control">
                                                <option value="" selected>Select Type</option>
                                                <option value="PUB">Public</option>
                                                <option value="PRI">Private</option>
                                                <option value="OTH">others</option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="form-row">
                                        <div class="col-md-8"></div>
                                        <div class="col-md-4">
                                            <label></label>
                                            <br/>
                                            <br/>

                                            <div class="form-group">
                                                <button type="submit" class="btn btn-base float-right">
                                                    Search
                                                    <i class="fa fa-search"></i>&nbsp;
                                                </button>
                                            </div>
                                        </div>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <div class="table-responsive">
                                    <table class="table table-striped">
                                        <thead>
                                        <tr>
                                            <th scope="col">SL</th>
                                            <th scope="col">Purpose</th>
                                            <th scope="col">Type</th>
                                            <th scope="col">Start Date</th>
                                            <th scope="col">End Date</th>
                                            <th scope="col">Day</th>
                                            <th scope="col">Branch</th>
                                            <th scope="col" width="80px">Action</th>
                                        </tr>
                                        </thead>
                                        <tbody>
                                        <tr v-for="(holiday, SL) in holidays" :key="holiday.id">
                                            <td>{{ SL + 1 }}</td>
                                            <td>{{ holiday.holiday.purpose }}</td>
                                            <td>{{ holiday.holiday.type }}</td>

                                            <!--                                            <template v-for="holiday in holiday.holiday_branch_holiday" :key="holiday">-->
                                            <!--                                                <td>{{ holiday.start_date }}</td>-->
                                            <!--                                                <td>{{ holiday.end_date }}</td>-->
                                            <!--                                                <td>{{ holiday.day_count }} days</td>-->
                                            <!--                                                <td>{{ holiday.branch.branch_name }} </td>-->
                                            <!--                                            </template>-->
                                            <td>{{ holiday.start_date }}</td>
                                            <td>{{ holiday.end_date }}</td>
                                            <td>{{ holiday.day_count }} days</td>
                                            <td>{{ holiday.branch.branch_name }}</td>

                                            <td class="td_action">
                                                <router-link class="item"
                                                             :to="{ name: 'UpdateHoliday', params: { id: holiday.id } }">
                                                    <i class="fas fa-edit" style="color: #72e3da"></i>
                                                </router-link>

                                                <i class="fas fa-trash-alt" style="color: #b71c1c; padding: 10px"
                                                   @click="deleteHolidayItem(holiday.id) in holidays"> </i>
                                            </td>
                                        </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <br/>

                <div class="row">
                    <div class="col-md-4">
                        <ul class="pagination" v-if="pagination.count">
                            <li class="page-item">
                                <a href="#">Showing {{ pagination.showing }} of {{ pagination.count }}</a>
                            </li>
                            <li :class="{ disabled: !pagination.previous }" class="page-item">
                                <a href="#!" v-on:click="setPage(pagination.previous)">Previous</a>
                            </li>

                            <li :class="{ disabled: !pagination.next }" class="page-item">
                                <a href="#!" v-on:click="setPage(pagination.next)">Next</a>
                            </li>
                        </ul>
                    </div>
                    <div class="col-md-6"></div>
                    <div class="col-md-2"></div>
                </div>
            </div>
        </template>
    </Master>
</template>

<script>
    import Master from "../../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";
    // import Modal from "../Modal.vue";

    export default {
        name: "Holiday",
        components: {Master},
        data() {
            return {
                //   modalOpen: false,
                branch: null,
                purpose: null,
                type: null,
                all_branchs: null,
                holidays: null,
                pagination: {
                    count: null,
                    next: null,
                    previous: null,
                    showing: 0,
                    page: null,
                },
            };
        },

        created() {
            this.initialLoad();
            console.log("created");
        },
        updated() {
            this.initialLoad();
            console.log("updated");
        },

        methods: {
            initialLoad: function () {
                this.GetHolidayList();
                // this.resetOptions();
                this.loadBranch();
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
                        // this.all_branchs = response.data;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            GetHolidayList: function () {
                var queryParam = {
                    branch: this.$route.query.branch,
                    type: this.$route.query.type,
                    purpose: this.$route.query.purpose,
                    page: this.$route.query.page,
                };
                axios
                    .get("branch-holidays/", {
                        params: queryParam,
                    })
                    .then((response) => {
                        this.holidays = response.data.results;

                        console.log("////", response.data.results);

                        this.pagination.count = response.data.count;
                        this.pagination.next = response.data.next;
                        this.pagination.previous = response.data.previous;
                        this.pagination.showing = response.data.results.length;

                        console.log(this.holidays);
                        console.log(response.status);
                    });
            },

            deleteHolidayItem: function (id) {
                Swal.fire({
                    title: "Are you sure?",
                    text: "You won't be able to revert this!",
                    icon: "warning",
                    showCancelButton: true,
                    confirmButtonColor: "#3085d6",
                    cancelButtonColor: "#d33",
                    confirmButtonText: "Yes, delete it!",
                }).then((response) => {
                    if (response.isConfirmed) {
                        axios.delete("branch-holidays/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetHolidayList();
                            }
                            // Swal.fire("Deleted!", "Holiday has been deleted!!", "success");
                        });
                        // Swal.fire("Cancelled", "Holiday has not been deleted !", "error");
                        Swal.fire("Deleted!", "Holiday has been deleted!!", "success");
                    } else {
                        Swal.fire("Cancelled", "Holiday has not been deleted !", "error");
                    }
                });
            },

            setPage(page) {
                this.pagination.page = page;
                this.searchHoliday();
            },

            searchHoliday() {
                console.log(this.searchName);
                console.log(this.searchMail);
                this.$router.push({
                    path: "branch-holiday",
                    query: {
                        branch: this.branch,
                        purpose: this.purpose,
                        type: this.type,

                        page: this.pagination.page,
                    },
                });

                // this.loadUsers();
                // this.$router.push("/user-list");
            },
        },
    };
</script>

<style>
    .table {
        text-align: left;
    }

    .td_action button {
        margin: 3px;
    }

    .btn-base {
        color: #fff;
        background-color: #3ac47d;
        border-color: #3ac47d;
        margin-top: 2px;
    }

    .headerContaier {
        margin: 0px 20px 20px 20px;
    }

    .card {
        margin-top: 20px;
        padding-top: 20px;
        margin-bottom: 20px;
    }
</style>
