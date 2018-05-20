<template>
  <div class="container">
    <div class="row">
      <div class="col-xs-12">
        <h2><strong>myBonsai</strong></h2>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-12">
        <router-link :to="{name: 'userBonsai-add'}"><button type="button" class="btn btn-raised btn-primary">Añadir Bonsai</button></router-link>
        <ul id="userBonsai-list" v-if="userBonsai != null">
          <li v-for="bonsai in userBonsai">
            <strong>{{bonsai.alias}}</strong>
            <strong>{{bonsai.iduserbonsai}}</strong>
            <p>
              <router-link :to="{name: 'logCuidados', params: {id: bonsai.iduserbonsai}}">Cuidados</router-link>
              <router-link :to="{name: 'userBonsai-detail', params: {id: bonsai.iduserbonsai}}">Detalle</router-link>
              <router-link :to="{name: 'userBonsai-edit', params: {id: bonsai.iduserbonsai}}">Editar</router-link>
              <span v-if="showRemove != bonsai.iduserbonsai">
						<a @click="removeUserBonsai(bonsai.iduserbonsai)" style="cursor:pointer;">Eliminar</a>
					</span>
              <span v-else>
            <p>¿Estas seguro de querer borrarlo?</p>
            <button @click="cancelRemove()">Cancelar</button>
            <button @click="confirmRemove(bonsai.iduserbonsai)">Borrar</button>
            </span>
            </p>
          </li>
        </ul>
        <span v-else>Cargando datos...</span>
      </div>
    </div>
  </div>

</template>

<script>
  import axios from 'axios'
  export default {
    name: "userBonsai-list",
    mounted() {
      let router = this.$router;
      let token = localStorage.getItem("token");
      if (!token && token == 'undefined'){
        router.push('/login');
      }
      this.getUserBonsai();
    },
    data() {
      return {
        texto: 'Componente de userBonsai',
        userBonsai: null,
        showRemove: null
      }
    },
    methods: {
      getUserBonsai(){
        let token = localStorage.getItem("token");
        let params = 'authorization='+token;
        axios.post('http://localhost:8000/userBonsai/list', params)
          .then((respuesta) => {
            this.userBonsai = respuesta.data.data;
          })
          .catch((error)=>{
            // console.log(error);
          });

      },
      removeUserBonsai(id) {
        this.showRemove = id;
      },
      cancelRemove() {
        this.showRemove = null;
      },
      confirmRemove(id){
        let token = localStorage.getItem("token");
        let params = 'authorization='+token;
        axios.post('http://localhost:8000/userBonsai/remove/'+id, params)
          .then((respuesta) => {
            this.showRemove = null;
            this.getUserBonsai();
          })
      }
    },
  }
</script>
