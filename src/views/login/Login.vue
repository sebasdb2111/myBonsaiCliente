<template>
  <div id="login">
    <h2>Login</h2>
    <router-link  :to="{name: 'registro'}"><button type="button" class="btn btn-raised btn-primary">Registrarme</button></router-link>
    <form v-on:submit.prevent="login">
      <p>
        <label>Email</label>
        <input type="text" v-model="UserLogin.email">
      </p>
      <p>
        <label>Password</label>
        <input type="password" v-model="UserLogin.password">
      </p>
      <input type="submit" value="Login">
    </form>
  </div>
</template>

<script>
  import axios from 'axios';
  export default {
    name: "login",
    mounted(){

    },
    data(){
      return {
        UserLogin: {
          email: null,
          password: null,
        }
      };
    },
    methods: {
      login(){
        let router = this.$router;
        let json = JSON.stringify(this.UserLogin);

        let params = 'json='+json;
        axios.post('http://localhost:8000/login', params)
          .then((respuesta) => {
            if (respuesta.data.satus !== 'error'){
              localStorage.setItem("token", respuesta.data);
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
