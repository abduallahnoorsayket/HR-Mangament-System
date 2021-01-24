<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="pe-7s-graph2 icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Rating Point List</div>
              <router-link :to="{ path: '/create-rating-point' }">
                <a class="float" style="position: absolute; right: 0">
                  <button type="button" class="btn btn-success" style="font-size: 15px">
                    <span class="fas fa-plus-square"></span> Add Rating Point
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
                      <th scope="col">Rating Point Name</th>
                      <th scope="col">Rating Point Value</th>
                      <th scope="col">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(rating, index) in all_rating_points" :key="index">
                      <td>{{ index + 1 }}</td>
                      <td>{{ rating.name }}</td>
                      <td><span class="badge badge-pill badge-info">{{ rating.value }}</span> </td>

                      <td class="td_action">
                        <router-link class="item" :to="{
                            name: 'EditRatingPoint',
                            params: { id: rating.id },}">
                          <i class="fas fa-edit" style="color: #72e3da; font-size: 20px" ></i>
                        </router-link>
                        <i class="fas fa-trash-alt" style="color: firebrick; font-size: 20px; padding: 10px" @click="
                            deleteRatingPoint(rating.id) in all_rating_points ">
                        </i>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </template>
  </Master>
</template>

<script>
import Master from "../../../Master";
import axios from "axios";
import Swal from "sweetalert2";
import "../../../authheader";
export default {
  name: "RatingPointList",
  components: { Master },
  data() {
    return {
      all_rating_points: null,
    };
  },
  created() {
    this.GetRatingPointList();
  },
  methods: {
    GetRatingPointList: function () {
      const token = localStorage.getItem("token");
      axios.get("rating-points/",{ headers: {
            Authorization: `token ${token}`,
          },}).then((response) => {
        this.all_rating_points = response.data;
      });
    },

    deleteRatingPoint: function (id) {
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
          axios.delete("rating-points/" + id + "/").then((response) => {
            if (response.status === 204) {
              this.GetRatingPointList();
            }
          });
          Swal.fire("Deleted!", "Rating point has been deleted!!", "success");
        } else {
          Swal.fire(
            "Cancelled",
            "Rating point has not been deleted !",
            "error"
          );
        }
      });
    },
  },
};
</script>

<style scoped>
.table {
  text-align: center;
}

.td_action button,i {
  margin: 3px;
  cursor: pointer;
}
</style>