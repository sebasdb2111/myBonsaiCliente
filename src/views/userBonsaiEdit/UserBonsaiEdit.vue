<template>
  <div id="useBonsai-edit">
    <h2>Editar bonsai</h2>
    <form v-on:submit.prevent="guardarUserBonsai">
      <p>
        <label>Tipo</label>
        <select v-model="UserBonsai.idbonsai.idbonsai">
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
        <input type="text" v-model="UserBonsai.fechaadquisicion.timestamp">
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
    name: "UserBonsaiEdit",
    mounted(){
      this.id = this.$route.params.id;
      let token = localStorage.getItem("token");
      let params = 'authorization='+token;
      axios.post('http://localhost:8000/userBonsai/detail/' + this.id, params)
        .then((respuesta) => {
          this.UserBonsai = respuesta.data.data;
        });

    },
    data(){
      return {
        id: null,
        UserBonsai: {
          idbonsai: null,
          alias: '',
          edad: null,
          fechaadquisicion: '',
          descripcion: '',
        }
      };
    },
    methods: {
      guardarUserBonsai(){
        let router = this.$router;
        let id = this.id;
        let token = localStorage.getItem("token");
        let json = JSON.stringify(this.UserBonsai);
        let params = 'json='+json+'&authorization='+token;
        //let params = '&json='+JSON.stringify(this.UserBonsai);

        axios.post('http://localhost:8000/userBonsai/edit/'+id, params)
          .then((respuesta) => {
            if (respuesta.data.status == 'success'){
              router.push('/detail/'+id);
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
