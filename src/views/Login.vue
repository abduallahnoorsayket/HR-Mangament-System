<template>
  <div>
    <div class="loginContainer">
      <div class="container">
        <div class="row">
          <div class="col-md-4"></div>
          <div class="col-md-8"></div>
        </div>
        <div class="row">
          <div class="col-md-8">
            <div class="ImageContainer">
              <!-- <img src="./login01.png" alt="no image" /> -->
              <div id="carouselExampleControls1" class="carousel slide" data-ride="carousel">
                <div class="carousel-inner">
                  <div class="carousel-item active">
                    <img src="./login01.png" alt="First slide" />
                  </div>
                  <div class="carousel-item">
                    <img src="./login02.png" alt="Second slide" />
                  </div>
                  <div class="carousel-item">
                    <img src="./login03.png" alt="Third slide" />
                  </div>
                </div>
                <a class="carousel-control-prev" href="#carouselExampleControls1" role="button" data-slide="prev">
                  <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                  <span class="sr-only">Previous</span>
                </a>
                <a class="carousel-control-next" href="#carouselExampleControls1" role="button" data-slide="next">
                  <span class="carousel-control-next-icon" aria-hidden="true"></span>
                  <span class="sr-only">Next</span>
                </a>
              </div>
            </div>
          </div>
          <div class="col-md-4">
            <div class="form-container">
              <h3 class="text-center mb-4 ml-4">Welcome Back !!</h3>
              <p class="formDescription">To keep connected with us please login with your personal information by email address and password</p>
              <div class="formInput">
                <form style="max-width: 500px; margin: auto" action="#" @submit.prevent="handleSubmit">
                  <div class="input-container">
                    <i class="fa fa-envelope icon"></i>
                    <input class="input-field" type="email" placeholder="Email" name="email" v-model="username" />
                  </div>

                  <div class="input-container">
                    <i class="fa fa-key icon"></i>
                    <input class="input-field" type="password" placeholder="Password" name="psw" v-model="password" />
                  </div>

                  <button type="submit" class="btn">Sign in</button>
                  <!--                <p class="forgot mt-4" align="center">-->
                  <!--                  <a href="#">Forgot Password?</a>-->
                  <!--                </p>-->
                </form>
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
import * as Swal from "sweetalert2";

export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    handleSubmit() {
      axios
        .post("login/", {
          username: this.username,
          password: this.password,
        })
        .then((response) => {
          console.log(response);
          localStorage.setItem("id", response.data.id);
          localStorage.setItem("username", response.data.username);
          localStorage.setItem("first_name", response.data.first_name);
          localStorage.setItem("last_name", response.data.last_name);
          localStorage.setItem("active_status", response.data.active_status);
          localStorage.setItem("user_profile", JSON.stringify(response.data.user_profile));
          localStorage.setItem("branch_weekly_holiday", JSON.stringify(response.data.user_profile.branch.branch_weekly_holiday));
          localStorage.setItem("superuser_status", response.data.superuser_status);
          localStorage.setItem("groups", JSON.stringify(response.data.groups));
          localStorage.setItem("user_permissions", JSON.stringify(response.data.user_permissions));
          localStorage.setItem("token", response.data.token);
          // localStorage.setItem("image", response.data.image);

          // this.$router.push("/dashboard");
          window.location.href = "/dashboard";
        })
        .catch((error) => {
          Swal.fire({
            icon: "error",
            text: "Provided credentials are not correct",
          }); // swal
          console.log(error);
        }); // catch
    },
  },
};
</script>

<style scoped>
body {
  font-family: Arial, Helvetica, sans-serif;
}

* {
  box-sizing: border-box;
}

.loginContainer {
  height: 100vh;
  display: flex;
  align-items: center;
  background: #fdfdfd;
}

.form-container {
  margin-top: 70px;
  margin-left: 10px;
}

.formInput {
  margin-top: 50px;
}

.formDescription {
  margin: 20px 20px 40px 20px;
  text-align: center;
}

.ImageContainer {
  /* margin-top: ; */
  margin: 100px 10px;
}

h3 {
  color: #cc99ff;
}

.input-container {
  display: -ms-flexbox; /* IE10 */
  display: flex;
  width: 100%;
  margin-bottom: 15px;
}

.carousel-inner img {
  width: 100%;
  height: 100%;
}

.icon {
  padding: 10px;
  margin-top: 5px;

  color: black;
  min-width: 50px;
  text-align: center;
}

.input-field {
  width: 100%;
  padding: 10px;
  outline: none;
  border-top: none;
  border-left: none;
  border-right: none;
}
/* 
.input-field:focus {
  border: 2px solid dodgerblue;
} */

/* Set a style for the submit button */
.btn {
  float: right;
  display: -webkit-box;
  display: -webkit-flex;
  display: -moz-box;
  display: -ms-flexbox;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0 10px;
  min-width: 160px;
  height: 50px;
  background-color: #cc99ff;
  border-radius: 25px;
  font-family: Poppins-Regular;
  font-size: 16px;
  color: #fff;
  line-height: 1.2;
  -webkit-transition: all 0.4s;
  -o-transition: all 0.4s;
  -moz-transition: all 0.4s;
  transition: all 0.4s;
  margin-top: 50px;
  cursor: pointer;
  width: 90%;

  /* background-color: #cc99ff;
  color: white;
  padding: 15px 20px;
  border: none;
  cursor: pointer;
  width: 100%;
  opacity: 0.9;
    border: none;
  border-bottom: solid 1px rgba(0, 0, 0, 0.1);
  margin-top: 20px;
   */
}

.btn:hover {
  opacity: 1;
}

.forgot {
  text-shadow: 0px 0px 3px rgba(117, 117, 117, 0.12);
  color: #e1bee7;
  padding-top: 15px;
  margin-top: 50px !important;
}

/* @import "../main.css";
    @import url("https://fonts.googleapis.com/css?family=Montserrat:400,800");

    body {
        background-color: #F3EBF6;
        font-family: 'Ubuntu', sans-serif;
    }

    .main {
        background-color: #FFFFFF;
        width: 400px;
        height: 400px;
        margin: 13em auto;
        border-radius: 1.5em;
        box-shadow: 0px 11px 35px 2px rgba(0, 0, 0, 0.14);
    }

    .sign {
        padding-top: 40px;
        color: #8C55AA;
        font-family: 'Ubuntu', sans-serif;
        font-weight: bold;
        font-size: 23px;
    }

    .un {
        width: 76%;
        color: rgb(38, 50, 56);
        font-weight: 700;
        font-size: 14px;
        letter-spacing: 1px;
        background: rgba(136, 126, 126, 0.04);
        padding: 10px 20px;
        border: none;
        border-radius: 20px;
        outline: none;
        box-sizing: border-box;
        border: 2px solid rgba(0, 0, 0, 0.02);
        margin-bottom: 50px;
        margin-left: 46px;
        text-align: center;
        margin-bottom: 27px;
        font-family: 'Ubuntu', sans-serif;
    }

    form.form1 {
        padding-top: 40px;
    }

    .pass {
        width: 76%;
        color: rgb(38, 50, 56);
        font-weight: 700;
        font-size: 14px;
        letter-spacing: 1px;
        background: rgba(136, 126, 126, 0.04);
        padding: 10px 20px;
        border: none;
        border-radius: 20px;
        outline: none;
        box-sizing: border-box;
        border: 2px solid rgba(0, 0, 0, 0.02);
        margin-bottom: 50px;
        margin-left: 46px;
        text-align: center;
        margin-bottom: 27px;
        font-family: 'Ubuntu', sans-serif;
    }

    .un:focus, .pass:focus {
        border: 2px solid rgba(0, 0, 0, 0.18) !important;
    }

    .submit {
        cursor: pointer;
        border-radius: 5em;
        color: #fff;
        background: linear-gradient(to right, #9C27B0, #E040FB);
        border: 0;
        padding-left: 40px;
        padding-right: 40px;
        padding-bottom: 10px;
        padding-top: 10px;
        font-family: 'Ubuntu', sans-serif;
        margin-left: 35%;
        font-size: 13px;
        box-shadow: 0 0 20px 1px rgba(0, 0, 0, 0.04);
    }

    .forgot {
        text-shadow: 0px 0px 3px rgba(117, 117, 117, 0.12);
        color: #E1BEE7;
        padding-top: 15px;
    }

    a {
        text-shadow: 0px 0px 3px rgba(117, 117, 117, 0.12);
        color: #E1BEE7;
        text-decoration: none
    }

    @media (max-width: 600px) {
        .main {
            border-radius: 0px;
        }
    } */
</style>
