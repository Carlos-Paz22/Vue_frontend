<template>


    <div class="row p-2 bg-white sticky-top border-bottom">
        <div class="col-1 align-self-center">
            <a class="btn  btn-link btn-sm rounded-circle text-danger" href="/Mis-imagenes" role="button">
                <svg width="1.6em" height="1.6em" viewBox="0 0 16 16" class="bi bi-arrow-left-circle" fill="currentColor" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" d="M8 15A7 7 0 1 0 8 1a7 7 0 0 0 0 14zm0 1A8 8 0 1 0 8 0a8 8 0 0 0 0 16z"/>
                    <path fill-rule="evenodd" d="M12 8a.5.5 0 0 1-.5.5H5.707l2.147 2.146a.5.5 0 0 1-.708.708l-3-3a.5.5 0 0 1 0-.708l3-3a.5.5 0 1 1 .708.708L5.707 7.5H11.5a.5.5 0 0 1 .5.5z"/>
                </svg>
            </a>
        </div>
        <div class="col-11 align-self-center text-center">
            <font face = "Forte" size = "7">Crear imagen</font>
        </div>
        <hr>
         <div class="row">
        <div class="col-12 col-md-6 text-center mx-auto">
            <form
            @submit.prevent="subirimagen"
            @change="prevista"
            enctype="multipart/form-data">
                <input type="file" ref="file" id="file" accept="image/*" required>
                &nbsp;
                <button type="submit" class="btn btn-success btn-sm">subir</button>
            </form>
            <span>Seleccione una categoria: </span>
            <div v-if="alerta === true">
                <div class="alert alert-danger" role="alert">
                    Seleccione almenos una categoria
                </div>
            </div>
            <div v-for="tag in cat" :key="tag.id">
                <input type="checkbox" :id="tag.nombre" :value="tag.id" v-model="checkedNames" >
                <label :for="tag.nombre" >{{ tag.nombre }}</label>
            </div>
        </div>
        <div class="col-12 col-md-6 text-center justify-content-center  mx-auto" style="height: 500px;">
            <p><b>Previsualizacion:</b></p>
            <br>
            <div v-if="image === ''">
                <div class="alert alert-danger" role="alert">
                    No a seleccionado ningun archivo
                </div>
            </div>
            <div>
                <img class="card-img-top" :src="image" alt="" >
            </div>
        </div>
    </div>
    </div>
  
   

</template>

<script>
import axios from 'axios';

export default {
    name: 'Subir',
    data(){
        return{
            cat: [],
            checkedNames: [],
            image: "",
            subirimage: "",
            alerta: false
        }
    },
    mounted() {
        axios.get('http://localhost:1337/tags').then((response) => {
            this.cat = response.data
        })
    },
    methods:{
        prevista(e){
            this.image = URL.createObjectURL(e.target.files[0])
            this.subirimage = e.target.files[0]
        },
        subirimagen(){
            if(this.checkedNames.length == 0)
            {
                this.alerta = true
            }else{
                this.alerta = false
                const token = localStorage.getItem('token')
                var formData = new FormData();
                let data = { tags: this.checkedNames }

                formData.append("files.imagen", this.subirimage);
                formData.append("data", JSON.stringify(data));
                console.log(this.subirimage)
                axios.post("http://localhost:1337/imagenes", formData ,{
                    headers:{
                        'Authorization': 'Bearer ' + token,
                        "Content-Type": 'multipart/form-data',
                    }
                }).then((response) => {
                    this.$router.push("/")
                })
            }
        }
    }
}

</script>