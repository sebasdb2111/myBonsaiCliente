<template>
  <div id="registro">
    <h2>Registrarse</h2>
    <!--{"username":"sebas","email":"sebas@sebas.com","password":"sebas",
    "nombre":"Sebastian","apellidos":"Diez Buades","fechaNacimiento":"21-11-1986"}-->
    <form v-on:submit.prevent="guardarUser">
      <p>
        <label>Username</label>
        <input type="text" v-model="User.username">
      </p>
      <p>
        <label>Email</label>
        <input type="text" v-model="User.email">
      </p>
      <p>
        <label>Password</label>
        <input type="password" v-model="User.password">
      </p>
      <p>
        <label>Nombre</label>
        <input type="text" v-model="User.nombre">
      </p>
      <p>
        <label>Apellidos</label>
        <input type="text" v-model="User.apellidos">

      </p><p>
      <label>Fecha de nacimiento</label>
      <input type="date" class="datepicker" v-model="User.fechaNacimiento">
    </p>
      <input type="submit" value="Registrarse">
    </form>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: "registro",
    mounted(){
      // Data Picker Initialization
      $('.datepicker').pickadate({
        //   dateFormat: 'dd-mm-yyy'
      });
    },
    data(){
      return {
        User: {
          username:null,
          email: '',
          password: null,
          nombre: '',
          apellidos: '',
          fechaNacimiento: '',
        }
      };
    },
    methods: {
      guardarUser(){
        let router = this.$router;

        let json = JSON.stringify(this.User);
        let params = 'json='+json;
        axios.post('http://localhost:8000/user/new', params)
          .then((respuesta) => {
            if (respuesta.data.status == 'success'){
              router.push('/login');
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
