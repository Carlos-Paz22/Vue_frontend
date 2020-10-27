<template>
<div>
  
</div>
  
</template>
<script>
import axios from "axios";
export default {
    name:'CreateTag',  
    data() {
        return {
            nombre:' '
        }
    },
    methods:{
        guardar(){
            axios.post("http://localhost:1337/tags",{
                 nombre: this.nombre 
            },{
                headers:{
                    'Authorization': 'Bearer ' + localStorage.getItem('token'),
                }
            })
            .then((response)=>{
                this.$router.push("/tags")
            });
        }
    }

}
</script>