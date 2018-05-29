<template>
  <div>
    <EdgeHeader color="teal darken-1"/>
    <div id="useBonsai-edit" class="container free-bird">
      <row>
        <column md="10" class="mx-auto float-none white z-depth-1 py-2 px-2">
          <card-body>
            <h2 class="h2-responsive"><strong>Nuevo Cuidado</strong></h2>
              <row>
                <column md="6">
                  <form v-on:submit.prevent="guardarUserBonsai">
                  <row>
                    <column md="4" class="md-form">
                      <p class="pull-right">Tipo</p>
                    </column>
                    <column md="8" class="md-form">
                      <select class="custom-select"  v-model="logCuidados.cuidado">
                        <option value="Regar">Regar</option>
                        <option value="Abonar">Abonar</option>
                        <option value="Pulverizar">Pulverizar</option>
                        <option value="Transplantar">Transplantar</option>
                      </select>
                    </column>
                  </row>
                  <row>
                    <column md="4" class="md-form">
                      <p class="pull-right">Fecha</p>
                    </column>
                    <column md="8" class="md-form">
                      <input type="date" id="materialFormRegisterDateEx" class="form-control md-input" v-model="logCuidados.createdAt">
                    </column>
                  </row>
                  <button class="btn btn-default margin-top-12 pull-right" type="submit">Editar</button>
                  </form>
                </column>
                <column md="6" class="border-left-3">
                  <table class="table table-hover w-auto">
                    <thead class="default-color lighten-4">
                    <tr>
                      <th></th>
                      <th>Fecha</th>
                      <th>Cuidado</th>
                      <th>Oopciones</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr>
                      <th scope="row">1</th>
                      <td>Regar</td>
                      <td>21-03-2018</td>
                      <td>
                        <button type="button" class="btn btn-warning btn-circle"><i class="fa fa-pencil fa-lg" aria-hidden="true"/></button>
                        <button type="button" class="btn btn-danger btn-circle"><i class="fa fa-trash-o fa-lg" aria-hidden="true"/></button>
                      </td>
                    </tr>
                    <tr>
                      <th scope="row">2</th>
                      <td>Abonar</td>
                      <td>11-03-2018</td>
                      <td>
                        <button type="button" class="btn btn-warning btn-circle"><i class="fa fa-pencil fa-lg" aria-hidden="true"/></button>
                        <button type="button" class="btn btn-danger btn-circle"><i class="fa fa-trash-o fa-lg" aria-hidden="true"/></button>
                      </td>
                    </tr>
                    <tr>
                      <th scope="row">3</th>
                      <td>Pulverizar</td>
                      <td>01-04-2018</td>
                      <td>
                        <button type="button" class="btn btn-warning btn-circle"><i class="fa fa-pencil fa-lg" aria-hidden="true"/></button>
                        <button type="button" class="btn btn-danger btn-circle"><i class="fa fa-trash-o fa-lg" aria-hidden="true"/></button>
                      </td>
                    </tr>
                    </tbody>
                  </table>
                </column>
              </row>
          </card-body>
        </column>
      </row>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import { Container, Column, Row, Fa, Navbar, NavbarItem, NavbarNav, NavbarCollapse, Btn, EdgeHeader, CardBody } from 'mdbvue';
  export default {
    name: "UserBonsaiEdit",
    components: {
      Container,
      Column,
      Row,
      Fa,
      Navbar,
      NavbarItem,
      NavbarNav,
      NavbarCollapse,
      Btn,
      EdgeHeader,
      CardBody
    },
    mounted(){
      this.id = this.$route.params.id;
      let token = localStorage.getItem("token");
      let params = 'authorization='+token;
      axios.post('http://localhost:8000/logCuidados/' + this.id, params)
        .then((respuesta) => {
          this.UserBonsai = respuesta.data.data;
        });
    },
    data(){
      return {
        id: null,
        logCuidados: {
          cuidado: '',
          createdAt: ''
        },
        showRemove: null
      };
    },
    methods: {
      guardarUserBonsai(){
        let id = this.id;
        let router = this.$router;
        let json = JSON.stringify(this.logCuidados);
        let token = localStorage.getItem("token");
        let params = 'json='+json+'&authorization='+token;

        axios.post('http://localhost:8000/logCuidados/new/'+id, params)
          .then((respuesta) => {
            if (respuesta.data.status == 'success'){
              router.push('/logCuidados/'+id);
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
  .margin-top-12 {
    margin-top: 12px;
  }

  .active-green-vue textarea.md-textarea, input.md-input{
    border-bottom: 1px solid #00897B;
    box-shadow: 0 1px 0 0 #00897B;
  }

  .padding-left-10 {
    padding-left: 10px;
  }

  .border-left-3 {
    border-left: 3px solid #00897B;
  }

  .btn-circle {
    width: 45px;
    height: 45px;
    padding: 10px 16px;
    font-size: 18px;
    line-height: 1.33;
    border-radius: 25px;
  }
  .w-auto {
    width: auto;
  }

</style>

