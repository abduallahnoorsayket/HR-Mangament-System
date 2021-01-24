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
                                My Evaluation
                            </div>
                        </div>
                    </div>
                </div>
                <div class="main-card card">
                    <div class="card-body">
                        <form @submit.prevent="SearchMyEvaluation">
                            <div class="form-group">

                                <div class="form-row">
                                    <div class="col-md-4">
                                        <div class="position-relative form-group">
                                            <label>Year</label>
                                            <select
                                                    name="select"
                                                    id="Year"
                                                    v-model="year_of_evaluation"
                                                    class="form-control">
                                                <option value="" selected>Select Year</option>
                                                <option v-for="year in all_years" :key="year.index"
                                                        :value="year.attendance_datetime__year" selected>
                                                    {{ year.attendance_datetime__year }}
                                                </option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="col-md-4">
                                        <div class="position-relative form-group">
                                            <label>Month</label>

                                            <select
                                                    name="select"
                                                    id="Month"
                                                    v-model="month_of_evaluation"
                                                    class="form-control">
                                                <option value="" selected>Select Month</option>
                                                <option v-for="item in attendance_months" :key="item.id"
                                                        :value="item.numeric">
                                                    {{ item.name }}
                                                </option>
                                            </select>
                                        </div>
                                    </div>

                                    <div class="col-md-4">
                                        <div class="position-relative form-group">
                                            <label>Graph Type</label>
                                            <select
                                                    name="select"
                                                    id="type"
                                                    v-model="graphType"
                                                    type="choice"
                                                    class="form-control">
                                                <option value="radar">Radar</option>
                                                <option value="bar">Bar</option>
                                                <option value="line">Line</option>
                                                <option value="area">Area</option>
                                            </select>
                                        </div>
                                    </div>
                                </div>

                                <div class="form-group">
                                    <button @submit.prevent="SearchMyEvaluation"
                                            class="mt-2 btn btn-success btn-lg float-right">
                                        Search <i class="fa fa-search"></i>
                                    </button>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>


                <!--Radar Chart-->

                <div v-if="myEvaluationData != null">
                    <div class="text-center card">
                        <h5>Data of {{ myEvaluationData[0].employee.first_name + ' ' +
                            myEvaluationData[0].employee.last_name}}
                        </h5>
                    </div>
                </div>


                <div class="row" v-if="myEvaluationData">
                    <div class="col-md-6" v-for="(allChart, index) in all_chartOptions" :key="index">
                        <template v-for="item in attendance_months" :key="item">
                            <p v-if="item.numeric === allChart.month_name" style="margin-bottom: -3px">
                                Month of: {{ item.name}}
                            </p>
                        </template>
                        {{allChart.remarks}}
                        <div class="card">
                            <VueApexCharts
                                    type="radar"
                                    height="400"
                                    :options="allChart"
                                    :series="all_series[index]">
                            </VueApexCharts>
                        </div>
                    </div>
                    <!--Export PDF-->
                    <button class="mt-5 mr-3 btn btn-success" style="height: 33px" @click="exportPdf">Export PDF</button>

                    <!--Export Excel-->
                    <button type="button" style="height: 33px" class="mt-5 btn btn-success" @click="exportExcel">Export Excel</button>
                </div>


            </div>
        </template>
    </Master>
</template>

<script>
    import Master from "../../../Master";
    import axios from "axios";
    import VueApexCharts from "vue3-apexcharts";

    // PDF library
    import jspdf from 'jspdf'
    import 'jspdf-autotable'

    // Excel library
    import XLSX from 'xlsx';

    export default {
        name: "MyEvaluation",
        components: {Master, VueApexCharts},

        data() {
            return {
                all_years: null,
                attendance_months: [
                    {name: "January", numeric: 1},
                    {name: "February", numeric: 2},
                    {name: "March", numeric: 3},
                    {name: "April", numeric: 4},
                    {name: "May", numeric: 5},
                    {name: "June", numeric: 6},
                    {name: "July", numeric: 7},
                    {name: "August", numeric: 8},
                    {name: "September", numeric: 9},
                    {name: "October", numeric: 10},
                    {name: "November", numeric: 11},
                    {name: "December", numeric: 12},
                ],
                myEvaluationData: null,

                year_of_evaluation: null,
                month_of_evaluation: null,
                graphType: null,

                all_series: {},
                all_chartOptions: {},


                /* Start Radar Chart */
                series: [
                    {
                        name: 'Series 1',
                        data: []
                    }
                ],

                chartOptions: {
                    chart: {
                        // height: 550,
                        type: 'radar',
                    },
                    title: {
                        // text: 'Radar Chart'
                    },
                    xaxis: {
                        categories: []
                    }
                },
                /* End Radar Chart */

            }

        },

        created() {
            this.loadYear()
        },

        updated() {
            // after finish work, myEvaluationData will be null
            this.myEvaluationData = null
            this.all_series = {}
            this.all_chartOptions = {}
        },

        methods: {

            // Load Year List
            loadYear() {
                const token = localStorage.getItem("token");
                axios
                    .get("years/", {
                        headers: {
                            Authorization: `token ${token}`,
                        },
                    })
                    .then((response) => {
                        this.all_years = response.data.data.years;
                    })
                    .catch(function (error) {
                        console.log(error);
                    });
            },

            // get Single Member Data
            getMyEvaluation: function () {
                var queryParam = {
                    employee: this.$route.query.employee,
                    year_of_evaluation: this.$route.query.year_of_evaluation,
                    month_of_evaluation: this.$route.query.month_of_evaluation,
                };
                axios
                    .get("employee-evaluation/", {
                        params: queryParam,
                    })
                    .then((response) => {
                        this.myEvaluationData = response.data;

                        // Nested api Iterate in Array from API
                        this.myEvaluationData.forEach(member_data => {

                            // Month wise Chart show
                            // Set data as key value pair. // key is Data property + parameter.Month Name // value is apexchart object/array like same is designed
                            this.all_series[member_data.month_of_evaluation] = [{
                                name: member_data.month_of_evaluation,
                                data: []
                            }]

                            this.all_chartOptions[member_data.month_of_evaluation] = {
                                chart: {

                                    type: this.graphType
                                    // type: 'radar'
                                },
                                title: {
                                    // text: 'Radar Chart'
                                },

                                remarks: 'Remarks: ' + member_data.additional_comments,

                                month_name: member_data.month_of_evaluation,

                                xaxis: {
                                    categories: []
                                }
                            }
                            member_data.employee_information_employee_rating.forEach(rating_value => {
                                this.all_chartOptions[member_data.month_of_evaluation].xaxis.categories.push(rating_value.rating.name)  //data property push into api key
                                this.all_series[member_data.month_of_evaluation][0].data.push(rating_value.rating_point.value)
                            })
                        });
                        // Nested api Iterate in Array from API End
                    });
            },

            // Search Department
            async SearchMyEvaluation() {
                let user_id = localStorage.getItem("id");
                await this.$router.push({
                    path: "my-evaluation",
                    query: {
                        year_of_evaluation: this.year_of_evaluation,
                        month_of_evaluation: this.month_of_evaluation,
                        employee: user_id,
                    },
                });
                this.getMyEvaluation();
            },

            // Export Pdf Method
            exportPdf() {
                let rating_point_column = ["Rating Name", "Rating Point", "Rating Value", "Evaluation Month"]
                let rating_point_name = []

                this.myEvaluationData.forEach(item => {
                    console.log('Item....', item.employee_information_employee_rating)
                    item.employee_information_employee_rating.map(element => {
                        let data = [element.rating.name, element.rating_point.value, element.rating_point.name, item.month_of_evaluation]
                        rating_point_name.push(data)
                    })
                })

                var doc = new jspdf('p', 'pt');
                doc.autoTable(rating_point_column, rating_point_name)
                doc.save('table.pdf');
            },

            // Export Excel Method
            exportExcel() {
                let rating_point = []

                this.myEvaluationData.forEach(item => {
                    console.log('Item....', item.employee_information_employee_rating)
                    item.employee_information_employee_rating.map(element => {
                        // console.log('ELEMENT....', element.rating)
                        rating_point.push({
                            Rating_Point_Name: element.rating.name,
                            Rating_Point_Value: element.rating_point.name,
                            Evaluation_Month: item.month_of_evaluation,
                        })
                    })
                })

                console.log('rating_point///', rating_point)
                const data = XLSX.utils.json_to_sheet(rating_point)
                const wb = XLSX.utils.book_new()
                XLSX.utils.book_append_sheet(wb, data)
                XLSX.writeFile(wb, 'demo.xlsx')
            },

        }
    }


</script>

<style scoped>


</style>