

<template>
  <div>
    <div class="fondo" style="max-height: 100%;">
      <div>
        <div class="text-center">
          <a href="/" title="Home — Unsplash">
            <img class="login__logo" src="../assets/login.png" />
          </a>
          <h1 class="login__title mt-4">Login</h1>
          <p class="login__subtitle">Welcome back.</p>
        </div>
      </div>

      <div class="flex-container mt-4">
        <div class="row mx-0">
          <div id="tamañoindex" class="container-fluid">
            <form @submit.prevent="login">
              <div>
                <label class="mt-3" for="email">Email</label> <br />
                <input
                  class="col-xs-12 col-sm-12 col-md-12 col-lg-12"
                  type="email"
                  v-model="email"
                  id="email"
                />
              </div>
              <div>
                <label for="password">Contraseña</label><br />
                <input
                  class="col-12"
                  type="password"
                  v-model="password"
                  id="password"
                />
                <br />
              </div>
              <div v-if="error">
                <div class="mt-2">
                  <b-alert
                    :show="dismissCountDown"
                    dismissible
                    variant="danger"
                    @dismissed="dismissCountDown = 0"
                    @dismiss-count-down="countDownChanged"
                  >
                    <p>Correo / Passowrd Incorrecto</p>
                    <b-progress
                      variant="danger"
                      :max="dismissSecs"
                      :value="dismissCountDown"
                      height="4px"
                    ></b-progress>
                  </b-alert>
                </div>
              </div>
              <div class="mt-4">

                <b-button    type="submit"  @click="showAlert"  id="color"    block    variant="dark"
                >
                  Ingresar
                </b-button>
                <div class="ingre mt-2 mb-2">
                  <a id="col_border" href="/registro">
                    <button
                      type="button"
                      id="color"
                      class="btn btn-dark btn-lg btn-block"
                    >
                      Registrarse
                    </button>
                  </a>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
      <br><br><br><br>
    </div>
    
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      error: false,
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

    login() {
      this.error = false;
      fetch("http://localhost:1337/auth/local/", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          identifier: this.email,
          password: this.password,
        }),
      })
        .then(async (response) => {
          if (!response.ok) {
            throw await response.json();
          }
          return response.json();
        })
        .then((data) => {
          localStorage.setItem("token", data.jwt);
          localStorage.setItem("user", JSON.stringify(data.user));
          this.$router.push("/");
        })
        .catch((err) => {
          this.error = true;
        });
    },
  },
};
</script>

<style >
.fondo {
  
  
  background-image: url('https://images8.alphacoders.com/718/718915.jpg');
  
  background-size: cover;

}

.login__logo {
  margin-top: 120px;
}

#tamañologin {
  background-color: rgb(255, 251, 251);
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
}
#col_border {
  text-decoration: none;
  color: white;
  margin-bottom: 50px;
}

#color {
  background: black;
}
.login__logo {
  width: 64px;
  height: 64px;
}

#tamañoindex {
  margin-top: 50px;
  width: 500px;
  max-height: 1000px;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
  background-color: rgb(255, 251, 251);
}

#color {
  background: black;
  margin-bottom: 50px;
}
</style>

