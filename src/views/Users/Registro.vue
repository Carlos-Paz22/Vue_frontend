<template>
<div style="max-height: 200%;">
 
    <div class="container-fluid mt-4 ">
      
      <div class="row  ">
        <div id="tamaño-regis" class="container-fluid ">
           <div class="text-center">
          <a href="/" >
            <img class="resitro" src="./assets/login.png" />
          </a>
        
        </div>
          <h1>Registro</h1>

          <form @submit.prevent="register">
            <label for="username">Usuario</label>
            <strong style="color: red"> * </strong>
            <br />
            <input  class="col-xs-12 col-sm-12 col-md-12 col-lg-12"   type="text"     size="50"    v-model="username"     required      />
            <br />
            <label for="nombre">Nombre </label>
            <strong style="color: red"> * </strong>
            <br />
           <input    type="text"    class="col-xs-12 col-sm-12 col-md-12 col-lg-12"       size="90"    v-model="nombre"    required  /> <br />
            <label for="email">Email </label>
            <strong style="color: red"> * </strong>
            <br />
            <input   type="email"    class="col-xs-12 col-sm-12 col-md-12 col-lg-12"    size="90"       v-model="email"      required      /><br />
           <label for="password">Password </label>
            <strong style="color: red"> * </strong>
            <br />
            <input   type="password"      class="col-xs-12 col-sm-12 col-md-12 col-lg-12"   size="50"     v-model="password"       required     /><br />

            <div v-if="error">
              <div class="mt-2">
                <b-alert
                  :show="dismissCountDown"
                  dismissible
                  variant="danger"
                  @dismissed="dismissCountDown = 0"
                  @dismiss-count-down="countDownChanged"
                >
                  <p><strong>Error</strong> Usuario / Correo Invalido</p>
                  <b-progress
                    variant="danger"
                    :max="dismissSecs"
                    :value="dismissCountDown"
                    height="4px"
                  ></b-progress>
                </b-alert>
              </div>
            </div>

            <div v-if="loading">
              <div class="mt-2">
                <b-alert
                  :show="dismissCountDown"
                  dismissible
                  variant="success"
                  @dismissed="dismissCountDown = 0"
                  @dismiss-count-down="countDownChanged"
                >
                  <p><strong>Success !</strong> Se Registro Correctamente</p>
                  <b-progress
                    variant="success"
                    :max="dismissSecs"
                    :value="dismissCountDown"
                    height="4px"
                  ></b-progress>
                </b-alert>
              </div>
            </div>
            <br />

            <b-button     type="submit"    @click="showAlert"     id="color" block          variant="dark"    onsubmit="setTimeout(function () { window.location.reload(); }, 10)"  >
              Ingresar
            </b-button>
          </form>
          <div id="mensaje"></div>
        </div>
      </div>
    </div>
 
</div>
  
</template>

<script>
import axios from "axios";

export default {
  name: "Registro",
  data() {
    return {
      username: "",
      nombre: "",
      email: "",
      password: "",
      error: false,
      loading: false,
      dismissSecs: 5,
      dismissCountDown: 0,
    };
  },

  methods: {
    countDownChanged(dismissCountDown) {
      this.dismissCountDown = dismissCountDown;
    },
    showAlert() {
      this.dismissCountDown = this.dismissSecs;
    },
    register() {
      this.error = false;
      this.loading = false;

      axios
        .post(
          "http://localhost:1337/auth/local/register/",

          {
            username: this.username,
            nombre: this.nombre,
            email: this.email,
            password: this.password,

            headers: {
              "Content-Type": "application/json",
            },
          }
        )
        .then((response) => {
          this.loading = true;
          // this.$router.push('/registro')
        })

        .catch((err) => {
          console.log("Fallo");
          this.error = true;
        });
    },
  },
};
</script>
<style >
.registroprincipal {
  background: aqua;
}
#col_border {
  text-decoration: none;
  color: white;
}

#color {
  background: black;
}
.login__logo {
  width: 64px;
  height: 64px;
}
/*  #tamaño {
            width: 500px;
            max-height: 1000px;
            
        } */

#color {
  background: black;
  margin-bottom: 50px;
}

#tamaño-regis {
  margin-top: 100px;
  width: 500px;
  max-height: 1000px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
  background-color: rgb(255, 251, 251);
}
.resitro{
  margin-top: 50px;
  margin-bottom: 50px;
   width: 64px;
  height: 64px;

}
/*  .fondo {
  
  
 
  
  
   background-image: url('https://images8.alphacoders.com/718/718915.jpg') no-repeat center center fixed; 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;

} */
</style>