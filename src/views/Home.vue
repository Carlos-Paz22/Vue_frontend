<template>
<div>

   <b-navbar  toggleable="md" type="dark" class="Nav"  >
    <b-navbar-brand href="/">Inicio</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        
        <b-nav-item href="/registro">Registro</b-nav-item>
         <b-nav-item href="/home" >Categorias</b-nav-item>
      </b-navbar-nav>

      <!-- Right aligned nav items -->
      <b-navbar-nav class="ml-auto">
        <b-nav-form>
          <b-form-input size="sm" class="mr-sm-2" placeholder="Search"></b-form-input>
          <b-button size="sm" class="my-2 my-sm-0" type="submit">Search</b-button>
        </b-nav-form>

        <b-nav-item-dropdown text="Lang" right>
          <b-dropdown-item href="#">EN</b-dropdown-item>
          <b-dropdown-item href="#">ES</b-dropdown-item>
          <b-dropdown-item href="#">RU</b-dropdown-item>
          <b-dropdown-item href="#">FA</b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown right>
          <!-- Using 'button-content' slot -->
          <template #button-content>
            <em>User</em>
          </template>
          <b-dropdown-item href="#">Profile</b-dropdown-item>
          <b-dropdown-item  @click="salir" >Sign Out</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>




   <h1>Bienvenido! {{user.username}}</h1>
    <h5>Correo:  {{user.email}}</h5>


 <div>
   <h1> Mis Imagenes</h1>

 </div>

  
  <div class="container mt-4">
    <div class="row">
      <div class="col-12"></div>
    </div>

     


           <input
            class=""
            type="file"
            @change="onFileSelected"
            accept="image/jpeg, image/png, image/jpg, image/gif"
          />
           <li class="btn ">
        <button class="btnbotonenvi" @click="onUpload" id="letra">ENVIAR IMAGEN</button>
           
      </li>
     
    


    <div class="row">
      <div class="col-12 col-sm-12 col-md-6 col-lg-4" v-for="items in galeria" :key="items.id">
       
        <Galeria :items="items" />

     <b-button   variant="danger" @click="eliminar(items.id)">Eliminar</b-button>
 
        
      </div>
    </div>
  </div>



</div>

</template>

<script>
// @ is an alias to /src
import Galeria from '@/components/Galeria.vue'
import axios from 'axios'

export default {
  name: 'Home',
  components: {
    Galeria
  },

   data() {
    return {
      galeria: [],
      user: {},
      selectedFile: null,
    }
  },


  mounted(){

    this.misimagenes();
  },

  methods: {

    salir (){
      localStorage.removeItem('token')
      localStorage.removeItem('user')
      this.$router.push('/')
    },
    //Enviar IMG
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
      this.url = URL.createObjectURL(this.selectedFile);
    },
      onUpload() {
      const fd = new FormData();
      fd.append( this.selectedFile, this.selectedFile.name);
      axios
        .post("http://localhost:1337/imagenes/", fd,
         {

          
        
      headers: {   
      'Authorization': 'Bearer ' +localStorage.getItem('token'),
    
     "Content-Type": "application/json",
         
      },
          
      
          
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        })

        .finally(() => (this.loading = true));

     
        
    },

    misimagenes(){
      
    this.user= JSON.parse(localStorage.getItem('user'))
    //recuperar el token para la Auth
    //const tokenverificacion = 
    fetch('http://localhost:1337/imagenes/me/',{
      headers: {
       'Authorization': 'Bearer ' +localStorage.getItem('token')
         
      }
    })
    .then( res=>res.json()
     
    )
    .then(data => {
      console.log(data)
      this.galeria = data
    })
    },

     eliminar(id) {
      axios
        .delete("http://localhost:1337/imagenes/" + id, {
          headers: {
            Authorization: "Bearer " + localStorage.getItem("token"),
          },
        })
        .then((response) => {
          this.misimagenes();
        });
    },

    
  },
  

  
 


};
</script>
