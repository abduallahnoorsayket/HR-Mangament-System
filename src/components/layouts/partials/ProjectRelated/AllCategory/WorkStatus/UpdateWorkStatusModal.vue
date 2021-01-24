<template>
    <teleport to="body">
        <div v-if="isUpdateVisible" class="modal">
            <div class="main-card card">
                <div class="card-header">
                    <p class="float-left">Update Work Status Name</p>
                </div>
                <div class="card-body" style="width: 600px">
                    <form @submit.prevent="updateWorkStatus" v-if="name">
                        <div class="form-group">

                            <label>Work Status Name</label>
                            <input placeholder="Status Name" id="work_type_name" v-model="name" type="text"
                                   class="form-control"/>


                            <div class="form-group">
                                <div class="form-row">
                                    <div class="col-md-6"></div>
                                    <div class="col-md-6">
                                        <button class="mt-2 float-right ml-2 btn btn-secondary"
                                                @click="CloseUpdateModal()">
                                            Close
                                        </button>
                                        <button @submit.prevent="updateWorkStatus"
                                                class="mt-2 btn btn-success float-right">
                                            Submit
                                        </button>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </teleport>
</template>

<script>
    import axios from "axios";
    import Swal from "sweetalert2";

    export default {
        name: "UpdateWorkTypeModal",
        props: ["employee_update"],
        data() {
            return {
                update_attendance_id: null,
                isUpdateVisible: false,

                name: null,
            };
        },
        created() {
            this.update_attendance_id = this.$props.employee_update.update_attendance_id;
            this.isUpdateVisible = this.$props.employee_update.isUpdateVisible;
            this.getWorkStatusData();
        },

        methods: {
            /*Close Modal Method*/
            CloseUpdateModal() {
                this.isUpdateVisible = !this.isUpdateVisible;
                this.$emit("close-update", this.isUpdateVisible);
            },

            /* get work status data */
            getWorkStatusData: function () {
                axios.get("work-statuses/" + this.update_attendance_id + "/").then(
                    (response) => {
                        this.name = response.data.name;
                    }, (response) => {
                        console.log("----", response);
                    }
                );
            },

            /* update Work status */
            updateWorkStatus() {
                axios
                    .put("work-statuses/" + this.update_attendance_id + "/", {
                        name: this.name,
                    })
                    .then((response) => {
                        Swal.fire({
                            icon: "success",
                            text: "You have successfully Edit Work Status ..",
                        }).then((response) => {
                            this.$router.push({name: "WorkStatus"});
                            this.$router.go();
                            console.log(response);
                        });

                        console.log(response);
                    })
                    .catch((error) => {
                        this.AddAttendance_error = error.response.data;
                        console.log("--++", error.response);
                    });
            },
        },
    };
</script>
<style>

</style>