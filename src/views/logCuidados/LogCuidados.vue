<template>
  <div id="logCuidados">
    <div>
      <h2>Crear nuevo cuidado</h2>
      <form v-on:submit.prevent="guardarLogCuidados">
        <p>
          <label>Cuidado</label>
          <select v-model="logCuidados.cuidado">
            <option></option>
            <option value="regar">Regar</option>
            <option value="abonar">Abonar</option>
            <option value="cortar">Cortar</option>
            <option value="transplantar">Transplantar</option>
          </select>
        </p>
        <p>
          <label>Fecha</label>
          <input type="text" v-model="logCuidados.createdAt">
        </p>
        <input type="submit" value="Guardar cuidado">
      </form>
    </div>
    <div>
      <ul v-if="logCuidados != null">
        <li v-for="cuidado in logCuidados">
          <p>{{cuidado.cuidado}}</p>
          <p>{{cuidado.createdat.timestamp}}</p>
          <span v-if="showRemove != cuidado.idlogcuidados">
          <a @click="removeLogCuidados(cuidado.idlogcuidados)" style="cursor:pointer;">Eliminar</a>
          </span>
          <span v-else>
          <p>¿Estas seguro de querer borrarlo?</p>
          <button @click="cancelRemove()">Cancelar</button>
          <button @click="confirmRemove(cuidado.idlogcuidados)">Borrar</button>
          </span>
        </li>
      </ul>
      <span v-else>Cargando datos...</span>
    </div>
  </div>

</template>

<script>
  import axios from 'axios';
  export default {
    name: "logCuidados",
    mounted(){
      this.getLogCuidados();
    },
    data(){
      return {
        logCuidados: {
          idlogcuidados: null,
          cuidado: '',
          createdAt: ''
        },
        showRemove: null
      };
    },
    methods: {
      getLogCuidados(){
        this.id = this.$route.params.id;
        let token = localStorage.getItem("token");
        let params = 'authorization='+token;
        axios.post('http://localhost:8000/logCuidados/'+this.id, params)
          .then((respuesta) => {
            this.logCuidados = respuesta.data.data;
          })
          .catch((error)=>{
            // console.log(error);
          });
      },
      guardarLogCuidados(){
        this.id = this.$route.params.id;
        let json = JSON.stringify(this.logCuidados);
        let token = localStorage.getItem("token");
        let params = 'json='+json+'&authorization='+token;
        console.log(params);

        axios.post('http://localhost:8000/logCuidados/new/'+this.id, params)
          .then((respuesta) => {
            if (respuesta.data.status == 'success'){
              router.push('/');
            }

          })
          .catch((error)=>{
            // console.log(error);
          });
      },
      removeLogCuidados(id) {
        this.showRemove = id;
      },
      cancelRemove() {
        this.showRemove = null;
      },
      confirmRemove(id){
        let token = localStorage.getItem("token");
        let params = 'authorization='+token;
        axios.post('http://localhost:8000/logCuidados/remove/'+id, params)
          .then((respuesta) => {
            this.showRemove = null;
            this.getLogCuidados();
          })
      }
    }
  }
</script>

<style scoped>

</style>
