<template>
  <div>
    <h1>Detalle del bonsai</h1>
    <div id="userBonsai-detail" v-if="userBonsaiDetail != null">
      <h2 v-text="userBonsaiDetail.alias"></h2>
      <p v-text="userBonsaiDetail.descripcion"></p>
    </div>
    <span v-else>Cargando datos...</span>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: "UserBonsaiDetail",
    mounted(){
      this.id = this.$route.params.id;
      let token = localStorage.getItem("token");
      let params = 'authorization='+token;
      axios.post('http://localhost:8000/userBonsai/detail/' + this.id, params)
        .then((respuesta) => {
          this.userBonsaiDetail = respuesta.data.data;
        })
        .catch((error)=>{
          // console.log(error);
        });
    },
    data(){
      return {
        id: null,
        userBonsaiDetail: null
      }

    }
  }
</script>

<style scoped>

</style>
