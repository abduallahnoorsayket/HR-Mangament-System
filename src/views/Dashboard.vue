<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrap">
            <!--Activity Buttons-->
            <div class="form-row">
              <div class="col-md-3">
                <div class="wrap">
                  <!--Login Text Show-->
                  <div class="card widget-content bg-happy-itmeo">
                    <div class="widget-content-wrapper text-white">
                      <div class="widget-content-left">
                        <div class="widget-heading">Login at</div>
                        <div class="widget-subheading">Good Morning</div>
                      </div>
                      <div class="widget-content-right">
                        <div class="widget-numbers text-white">
                          <span>{{ login }}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                  <!--Login Text Show-->
                  <div>
                    <button
                      @click.prevent="submitAttendanceEntry('login')"
                      type="button"
                      class="btn w-100 glow-on-hover"
                      :disabled="login">
                      Login at
                    </button>
                  </div>
                </div>
              </div>
              <div class="col-md-3">
                <div class="wrap">
                  <!--Breakin Text Show-->
                  <div class="card widget-content bg-love-kiss">
                    <div class="widget-content-wrapper text-white">
                      <div class="widget-content-left">
                        <div class="widget-heading">Break in at</div>
                        <div class="widget-subheading">Relax Time</div>
                      </div>
                      <div class="widget-content-right">
                        <div class="widget-numbers text-white">
                          <span>{{ break_in }}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                  <!--Breakin Text Show-->
                  <button
                    @click.prevent="submitAttendanceEntry('breakin')"
                    type="button"
                    class="btn w-100 glow-on-hover"
                    :disabled="break_in || !login"
                  >
                    Break in at
                  </button>
                </div>
              </div>
              <div class="col-md-3">
                <div class="wrap">
                  <!--Break out Text Show-->
                  <div class="card widget-content bg-happy-itmeo">
                    <div class="widget-content-wrapper text-white">
                      <div class="widget-content-left">
                        <div class="widget-heading">Break out at</div>
                        <div class="widget-subheading">Back to work</div>
                      </div>
                      <div class="widget-content-right">
                        <div class="widget-numbers text-white">
                          <span>{{ break_out }}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                  <!--Break out Text Show-->
                  <button
                    @click.prevent="submitAttendanceEntry('breakout')"
                    type="button"
                    class="btn w-100 glow-on-hover"
                    :disabled="break_out || !break_in"
                  >
                    Break out at
                  </button>
                </div>
              </div>
              <div class="col-md-3">
                <div class="wrap">
                  <!--Logout Text Show-->
                  <div class="card widget-content bg-love-kiss">
                    <div class="widget-content-wrapper text-white">
                      <div class="widget-content-left">
                        <div class="widget-heading">Logout at</div>
                        <div class="widget-subheading">Good Bye</div>
                      </div>
                      <div class="widget-content-right">
                        <div class="widget-numbers text-white">
                          <span>{{ logout }}</span>
                        </div>
                      </div>
                    </div>
                  </div>
                  <!--Logout Text Show-->
                  <button
                    @click.prevent="submitAttendanceEntry('logout')"
                    type="button"
                    class="btn w-100 glow-on-hover"
                    :disabled="logout || !break_out"
                  >
                    Logout at
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!--Normal User Data Table-->
        <EmployeeAttendanceEntry v-if="is_superuser()" />
        <AttendanceEntry v-if="attendance_list_render" />
      </div>
    </template>
  </Master>
</template>

<script>
import Master from "../components/layouts/Master";
import AttendanceEntry from "../components/layouts/partials/Attendance/AttendanceRecords/AttendanceEntry";
import EmployeeAttendanceEntry from "../components/layouts/partials/Attendance/AttendanceRecords/EmployeeAttendanceRecords";
import axios from "axios";
import Swal from "sweetalert2";
import permission from "../authorization/permissions";

export default {
  name: "Dashboard",
  components: { Master, AttendanceEntry, EmployeeAttendanceEntry },
  data() {
    return {
      section_name: null,

      login: null,
      break_in: null,
      break_out: null,
      logout: null,

      attendance_entry: null,
      today_attendance_record: null,

      attendance_list_render: true,
    };
  },

  methods: {
    is_superuser() {
      return permission.is_superuser();
    },
    // Login At
    async submitAttendanceEntry(section_name) {
      this.attendance_list_render = false;
      const token = localStorage.getItem("token");
      const response = await axios
        .post("attendance-entry/", {
          headers: {
            Authorization: `token ${token}`,
          },
          section_name: section_name,
        })
        .then((response) => {
          this.GetTodayAttendanceRecord();
          Swal.fire({
            icon: "success",
            // text: "You have successfully in" + {{ this.today_attendance_record}},
          }).then((result) => {
            console.log(result);
          });
          console.log(response);
        })
        .catch((error) => {
          console.log("--++", error.response);
        });
      console.log(response);
      this.attendance_list_render = true;
    },

    GetTodayAttendanceRecord: function () {
      axios.get("todays-attendance-record/").then((response) => {
        this.today_attendance_record = response.data;
        this.login = response.data.login_time;
        this.break_in = response.data.breakin_time;
        this.break_out = response.data.breakout_time;
        this.logout = response.data.logout_time;
        console.log(response.status);
      });
    },
  },

  created() {
    this.GetTodayAttendanceRecord();
  },
};
</script>


<style scoped>
@import "../main.css";

.glow-on-hover {
  width: 220px;
  height: 50px;
  border: none;
  outline: none;
  color: #fff;
  background: #111;
  cursor: pointer;
  position: relative;
  z-index: 0;
  font-size: 18px;
  border-radius: 10px;
}

.glow-on-hover:before {
  content: "";
  background: linear-gradient(
    45deg,
    #ff0000,
    #ff7300,
    #fffb00,
    #48ff00,
    #00ffd5,
    #002bff,
    #7a00ff,
    #ff00c8,
    #ff0000
  );
  position: absolute;
  top: -2px;
  left: -2px;
  background-size: 400%;
  z-index: -1;
  filter: blur(5px);
  width: calc(100% + 4px);
  height: calc(100% + 4px);
  animation: glowing 20s linear infinite;
  opacity: 0;
  transition: opacity 0.3s ease-in-out;
  border-radius: 10px;
}

.glow-on-hover:active {
  color: #000;
}

.glow-on-hover:active:after {
  background: transparent;
}

.glow-on-hover:hover:before {
  opacity: 1;
}

.glow-on-hover:after {
  z-index: -1;
  content: "";
  position: absolute;
  width: 100%;
  height: 100%;
  background: black;
  left: 0;
  top: 0;
  border-radius: 10px;
}

@keyframes glowing {
  0% {
    background-position: 0 0;
  }
  50% {
    background-position: 400% 0;
  }
  100% {
    background-position: 0 0;
  }
}

.user-name {
  color: #00c6ff;
  text-decoration: none;
}
</style>