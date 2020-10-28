<template >
  <div class="index">
    <!-- <input
      type="text"
      v-model="buscar"
      aria-label="Amount (to the nearest dollar)"
    /> -->
   

    <div class="container-flex">
      <b-navbar toggleable="md" type="dark" class="Nav">
        <b-navbar-brand href="/">Inicio</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <b-nav-item href="/login">Login</b-nav-item>
            <b-nav-item href="/registro">Registro</b-nav-item>
            <b-nav-item href="/home">Mi Perfil</b-nav-item>
             <div class="input-group-prepend">
      <select
        class="input-group-text bg-transparent text-black rounded-right"
        v-model="checkedNames"
      >
        <option
          class="text-dark"
          v-for="tag in tags"
          :key="tag.id"
          :value="tag.nombre"
        >
          {{ tag.nombre }}
        </option>
        <option class="text-dark" value="" selected>Todas</option>
      </select>
    </div>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-form>
              <b-form-input
                size="sm"
                class="mr-sm-2"
                 v-model="buscar"
                placeholder="Search"
              ></b-form-input>
              <b-button size="sm" class="my-2 my-sm-0" type="submit"
                >Search</b-button
              >
            </b-nav-form>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </div>

    <div class="container-flex">
      <b-carousel
        id="carousel-1"
        v-model="slide"
        :interval="4000"
        controls
        indicators
        background="#ababab"
        img-width="1024"
        img-height="480"
        style="text-shadow: 1px 1px 2px #333"
        @sliding-start="onSlideStart"
        @sliding-end="onSlideEnd"
      >
        <!-- Text slides with image -->
        <b-carousel-slide
          caption="First slide"
          text="Nulla vitae elit libero, a pharetra augue mollis interdum."
          img-src="https://picsum.photos/1024/480/?image=52"
          style="max-height: 400px"
        ></b-carousel-slide>

        <!-- Slides with custom text -->
        <b-carousel-slide
          img-src="https://picsum.photos/1024/480/?image=54"
          style="max-height: 400px"
        >
          <h1>Hello world!</h1>
        </b-carousel-slide>

        <!-- Slides with image only -->
        <b-carousel-slide
          img-src="https://picsum.photos/1024/480/?image=58"
          style="max-height: 400px"
        ></b-carousel-slide>

        <!-- Slides with img slot -->
        <!-- Note the classes .d-block and .img-fluid to prevent browser default image alignment -->
        <b-carousel-slide>
          <template #img>
            <img
              class="d-block img-fluid w-100"
              width="1024"
              height="480"
              src="https://picsum.photos/1024/480/?image=55"
              style="max-height: 400px"
              alt="image slot"
            />
          </template>
        </b-carousel-slide>
      </b-carousel>
    </div>

    <div class="container mt-4">
      <div class="row">
        <div class="col-12"></div>
      </div>

      <div class="row">
        <div
          class="col-12 col-sm-12 col-md-6 col-lg-4"
          v-for="items in galeria"
          :key="items.id"
        >
          <Galeria :items="items" />
           <b-button class="tamaño" @click="bajarImagen(items.imagen.url) " variant="outline-success">

        <b-icon icon="download"></b-icon> Descargar
    
      </b-button>
    <div v-if="Search === ''">
                <div class="alert alert-danger" role="alert">
                    No a seleccionado ningun archivo
                </div>
            </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Galeria from "@/components/Galeria.vue";
import axios from "axios";
import debounce from "debounce";

export default {
  name: "Home",
  components: {
    Galeria,

  },
  data() {
    return {
      galeria: [],
      user: {},
      slide: 0,
      sliding: null,
      tags: [],
      checkedNames: "",
      buscar: "",
      url:"",
    };
  },
  mounted() {
    axios.get("http://localhost:1337/tags").then((response) => {
      this.tags = response.data;
    }),
      (this.Search = debounce(this.Search, 600));

    this.traerimagen();
  },
  watch: {
    checkedNames(value) {
      this.Search(value);
    },
    buscar(value) {
      this.Search(value);
    },
  },

  methods: {
    Search(value) {
      axios
        .get("http://localhost:1337/imagenes?_sort=id%3ADESC", {
          params: {
            "tags.nombre_contains": value,
          },
        })
        .then((response) => {
          this.galeria = response.data;
        });
    },

    onSlideStart(slide) {
      this.sliding = true;
    },
    onSlideEnd(slide) {
      this.sliding = false;
    },

    traerimagen() {
      axios
        .get("http://localhost:1337/imagenes/", {
          headers: {
            "Content-Type": "application/json",
          },
        })
        .then((response) => {
          this.galeria = response.data;
          console.log(response);
        });
    },
    bajarImagen(imagenUrl) {
      this.url = "http://localhost:1337" + imagenUrl;
      axios({
        method: "get",
        url: this.url,
        responseType: "arraybuffer",
      })
        .then((response) => {
          this.forceFileDownload(response);
        })
        .catch(() => console.log("Fallo"));
    },
    
    forceFileDownload(response){
      const url = window.URL.createObjectURL(new Blob([response.data]))
      const link = document.createElement('a')
      link.href = url
      link.setAttribute('download', 'imagen.png') //or any other extension
      document.body.appendChild(link)
      link.click()
      },
    
    downloadWithAxios(){

      axios({
        method: 'get',
         url:   'http://localhost:1337'+tag.imagen.url,
        responseType: 'arraybuffer'
      })
      .then(response => {
        
        this.forceFileDownload(response)
        
      })
      .catch(() => console.log('Fallo al descargar'))
    }
  },
};
</script>




<style >
.Nav {
  background: rgb(39, 206, 218);
  width: auto;
}

#col_border {
  text-decoration: none;
}
h1 {
  color: rgb(0, 0, 0);
  text-align: center;
}

.color {
  background: black;
}

.btnhome {
  font-family: Lato, sans-serif;
  text-decoration: none;
  width: 1550px;
  border: 5px solid #2c3e50;
  color: #2c3e50;
  display: block;

  font-size: 2rem;
  letter-spacing: 0.1rem;
  padding: 1rem;
  position: relative;
  text-decoration: none;
  text-transform: uppercase;
}

.btnhome::before {
  content: "";
  background-color: #e26a6a;
  box-shadow: 10px 10px 0 #f1c40f, 20px 20px 0 #3498db;
  position: absolute;
  left: 0.25rem;
  top: 0.5rem;
  height: 102%;
  width: 102%;
  z-index: -1;
  transition: all 0.4s ease;
}

.btnhome:hover::before {
  box-shadow: none;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
}

.tamaño{
  margin-left: 90px;
}
</style>