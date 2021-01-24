<template>
    <Master>
        <template v-slot:content>
            <div class="app-main__inner">
                <div class="app-page-title">
                    <div class="page-title-wrapper">
                        <div class="page-title-heading">
                            <div class="page-title-icon">
                                <i class="fas fa-list-alt icon-gradient bg-tempting-azure"></i>
                            </div>
                            <div>
                                Holidays
                            </div>
                            <router-link :to="{ path: '/add-holiday' }">
                                <a class="float" style="position: absolute; right: 0">
                                    <button type="button" class="btn btn-success" style="font-size: 15px"><span
                                            class="fas fa-plus-square"></span> Add Holiday
                                    </button>
                                </a>
                            </router-link>
                        </div>
                    </div>
                </div>

                <div class="row">
                    <div class="col-12">
                        <div class="main-card card">
                            <div class="card-body">
                                <table class="table table-striped">
                                    <thead>
                                    <tr>
                                        <th scope="col">SL</th>
                                        <th scope="col">Purpose</th>
                                        <th scope="col">Holiday Tpe</th>
                                        <th scope="col">Action</th>
                                    </tr>
                                    </thead>
                                    <tbody>
                                    <tr v-for="(holiday, SL ) in holidays" :key="holiday.id">
                                        <td>{{SL+1}}</td>
                                        <td>{{holiday.purpose}}</td>
                                        <td>{{holiday.type}}</td>

                                        <td class="td_action">

                                            <router-link class="item"
                                                         :to="{ name: 'EditHoliday', params: { id: holiday.id } }">
                                                <i class="fas fa-edit" style="color: #72e3da"></i>
                                            </router-link>

                                            <i class="fas fa-trash-alt" style="color: firebrick; padding: 10px"
                                               @click="deleteHolidayItem(holiday.id) in holidays">
                                            </i>
                                        </td>
                                    </tr>
                                    </tbody>
                                </table>
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
    import Master from "../../../../Master";
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "HolidayList",
        components: {Master},
        data() {
            return {
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
        methods: {
            GetHolidaysList: function () {
                var queryParam = {
                    page: this.$route.query.page,
                };
                axios
                    .get("holidays/", {
                        params: queryParam,
                    })
                    .then((response) => {
                        this.holidays = response.data.results;

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
                        axios.delete("holidays/" + id + "/").then((response) => {
                            if (response.status === 204) {
                                this.GetHolidaysList();
                            }
                        });
                        Swal.fire("Deleted!", "Your holiday has been deleted!!", "success");
                    } else {
                        Swal.fire("Cancelled", "Your holiday has not been deleted !", "error");
                    }
                });
            },

            setPage(page) {
                this.pagination.page = page;
                this.searchHoliday();
            },
            async searchHoliday() {
                await this.$router.push({
                    path: "holiday-list",
                    query: {
                        page: this.pagination.page,
                    },
                });

                this.GetHolidaysList();
            },
        },

        created() {
            this.GetHolidaysList();
        },
    };
</script>

<style scoped></style>
