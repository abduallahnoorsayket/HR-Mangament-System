<template>
  <Master>
    <template v-slot:content>
      <div class="app-main__inner">
        <div class="app-page-title">
          <div class="page-title-wrapper">
            <div class="page-title-heading">
              <div class="page-title-icon">
                <i class="pe-7s-star icon-gradient bg-tempting-azure"></i>
              </div>
              <div>Rating List</div>
              <router-link :to="{ path: '/create-rating' }">
                <a class="float" style="position: absolute; right: 0">
                  <button type="button" class="btn btn-success" style="font-size: 15px">
                    <span class="fas fa-plus-square"></span> Add Rating
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
                      <th scope="col">Rating Name</th>
                      <th scope="col">Rating Status</th>
                      <th scope="col">Action</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="(rating, index) in all_ratings" :key="index">
                      <td>{{ index + 1 }}</td>
                      <td>{{ rating.name }}</td>
                      <td v-if="rating.status === true"><span class="badge badge-success">ON</span></td>
                      <td v-else><span class="badge badge-danger">OFF</span></td>
                      <td class="td_action">
                        <router-link class="item" :to="{name: 'EditRating', params: { id: rating.id },}">
                          <i class="fas fa-edit" style="color: #72e3da;  font-size: 20px "></i>
                        </router-link >
                        <i class="fas fa-trash-alt " style="color: firebrick; font-size: 20px; padding: 10px" @click="deleteRating(rating.id) in all_ratings">
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
  name: "RatingList",
  components: { Master },
  data() {
    return {
      all_ratings: null,
    };
  },
  created() {
    this.GetRatingList();
  },
  methods: {
    GetRatingList: function () {
      const token = localStorage.getItem("token");
      axios.get("ratings/",{ headers: {
            Authorization: `token ${token}`,
          },}).then((response) => {
        this.all_ratings = response.data;
      });
    },

    deleteRating: function (id) {
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
          axios.delete("ratings/" + id + "/").then((response) => {
            if (response.status === 204) {
              this.GetRatingList();
            }
          });
          Swal.fire("Deleted!", "Rating has been deleted!!", "success");
        } else {
          Swal.fire("Cancelled", "Rating  has not been deleted !", "error");
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