<template>
  <div id="useBonsai-add">
    <h2>Crear nuevo bonsai</h2>
    <form v-on:submit.prevent="guardarUserBonsai">
      <p>
        <label>Tipo</label>
        <select v-model="UserBonsai.idBonsai">
          <option></option>
          <option value="2">Manzano</option>
        </select>
      </p>
      <p>
        <label>Alias</label>
        <input type="text" v-model="UserBonsai.alias">
      </p>
      <p>
        <label>Fecha de adquisicion</label>
        <input type="text" v-model="UserBonsai.fechaAdquisicion">
      </p>
      <p>
        <label>Descripción</label>
        <textarea v-model="UserBonsai.descripcion"></textarea>
      </p>
      <p>
        <label>Edad</label>
        <select v-model="UserBonsai.edad">
          <option></option>
          <option  v-for="i in 100" :value="i">{{i}}</option>
        </select>
      </p>
      <input type="submit" value="Guardar bonsai">
    </form>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: "UserBonsaiAdd",
    mounted(){

    },
    data(){
      return {
        UserBonsai: {
          idBonsai:null,
          alias: '',
          edad: null,
          fechaAdquisicion: '',
          descripcion: '',
        }
      };
    },
    methods: {
      guardarUserBonsai(){
        let router = this.$router;
        let json = JSON.stringify(this.UserBonsai);
        let token = localStorage.getItem("token");
        let params = 'json='+json+'&authorization='+token;
        //let params = '&json='+JSON.stringify(this.UserBonsai);

        axios.post('http://localhost:8000/userBonsai/new', params)
          .then((respuesta) => {
            if (respuesta.data.status == 'success'){
              router.push('/');
            }

          })
          .catch((error)=>{
            // console.log(error);
          });
      }
    }
  }
</script>

<style scoped>

</style>
