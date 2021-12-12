<template>

  <div id="Transaction" class="transaction">
    <div class="container py-1">
      <nav class="navbar navbar-light bg-light">
        <div class="container-fluid">
          <img src="../assets/logito.png" alt="" width="80" height="38">
          <span class="navbar-brand mb-0 h1">FullSalas</span>
          <!-- Button trigger modal -->
          <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#staticBackdrop">
            Crear Sala
          </button>

          <!-- Modal -->
          <div class="modal fade" id="staticBackdrop" data-bs-backdrop="static" data-bs-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h5 class="modal-title" id="staticBackdropLabel">Crear Sala</h5>
                  <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                <form v-on:submit.prevent="procesoCrearSala">
                    <div class="mb-3">             
                      <input class="form-control" type="text" v-model="sala.id" placeholder="Id de la sala" required>              
                    </div>
                    <div class="mb-3">             
                      <input  class="form-control" type="text" v-model="sala.imagen" placeholder="Link de transmision" required>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.usernameAs" placeholder="Imagen" disabled>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.titulo" placeholder="Titulo" required>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.equipo1" placeholder="Equipo 1" required>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.equipo2" placeholder="Equipo 2" required>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.marcador1" placeholder="Marcador 2" disabled>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.marcador2" placeholder="Marcador 1" disabled>
                    </div>
                    <div class="mb-3 ">              
                      <input class="form-control" type="text" v-model="sala.tarifa" placeholder="Tarifa" required>
                    </div>
                      
                
                
                    <button type="submit" class="btn btn-primary">Crear Sala</button>
                 </form>
                </div>
                <div class="modal-footer">
                  <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                  
                </div>
              </div>
            </div>
          </div>
        </div>
      </nav>
    </div>
    <div class="container py-4">

      <div class="row row-cols-1 row-cols-md-3 g-4">
        <!--para traer todas las salas-->
        <div v-for="salas in allSalas" :key="salas.id">
        <div class="col">
          <div class="card">
            <iframe width="352" height="215" v-bind:src="`${salas.imagen}`" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
            <div class="card-body">
              <h5 class="card-title">{{ salas.titulo }}, ID: {{ salas.id }}</h5>              

              <button type="submit" class="btn btn-primary" @click="loadSala(`${salas.id}`)">Ver Apuesta</button>
                          
              <button type="button" class="btn btn-danger" data-bs-toggle="modal" data-bs-target="#exampleModal3">
                Eliminar Apuesta
              </button>

              
              <div class="modal fade" id="exampleModal3" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                            <div class="modal-dialog">
                                <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="exampleModalLabel">Eliminar</h5>
                                    
                                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                                </div>
                                <div class="modal-body">
                                    <form v-on:submit.prevent="eliminarSala">
                                      <div class="alert alert-danger" role="alert">
                                       Una vez eliminada la sala no hay vuelta atras
                                      </div>
                                      <div class="form-group">
                                      <input class="col-md-4 form-control" type="text" v-model="salaId" placeholder="ID sala a eliminar">
                                      </div>
                                      <br>
                                      <button type="submit" class="col-md-4 btn btn-danger">Eliminar apuesta</button>
                                    </form>
                                </div>
                                <div class="modal-footer">
                                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>                                
                                </div>
                                </div>
                            </div>
                        </div>
            </div>
          </div>
        </div>
        </div>    
      </div>

    </div>
   
  </div>

</template>


<script>
import gql from "graphql-tag";

export default {
  name: "Transaction",

  data: function() {
        return {
            allSalas: [],
            sala: {
              "id": "",
              "imagen": "",
              "usernameAs": localStorage.getItem("username"),
              "equipo1": "",
              "equipo2": "",
              "titulo": "",
              "equipoA": [],
              "equipoB": [],
              "marcador1": 0,
              "marcador2": 0,
              "cuenta": 0,
              "tarifa": ""
            },

            salaId: "",
        };
    },
    apollo: {
        allSalas: {
            query: gql`
            query AllSalas {
                allSalas {
                    id
                    imagen
                    usernameAs
                    equipo1
                    equipo2
                    titulo
                    equipoA
                    equipoB
                    marcador1
                    marcador2
                    cuenta
                    tarifa
                }
            }  
            `,
        }
    },

    created: function(){
        this.$apollo.queries.allSalas.refetch();
        //this.$emit("dataTransaction", this.$apollo.queries.allSalas.refetch());
    },

  methods:{
    
    //metodo para crear sala
    procesoCrearSala: async function() {

            if(localStorage.getItem("token_access") === null || localStorage.getItem("token_refresh") === null){
                this.$emit("loadLogout");
                return;
            }

            localStorage.setItem("token_access", "");

            //promesa para validar le usuario logeado, eso creo
            await this.$apollo
                .mutate({
                mutation: gql`
                    mutation CreateSala($refresh: String!) {
                        refreshToken(refresh: $refresh) {
                            access
                        }
                    }`,
                    variables: {
                        refresh: localStorage.getItem("token_refresh"),
                    },
                })
                .then((result) => {
                    localStorage.setItem("token_access", result.data.refreshToken.access);
                })
                .catch((error) => {
                    this.$emit("loadLogout");
                    return;
                })
            //promesa para crear la sala
            await this.$apollo
                .mutate({
                    mutation: gql`
                        mutation Mutation($sala: SalaInput!) {
                          createSala(sala: $sala) {
                          id  
                          imagen
                          usernameAs
                          equipo1
                          equipo2
                          titulo
                          equipoA
                          equipoB
                          marcador1
                          marcador2
                          cuenta
                          tarifa
                        }
                      }
                    `,
                    variables: {
                        sala: this.sala,
                    },
                })
                .then((result) => {
                    alert("Sala creada de manera correcta.");
                })
            .catch((error) => {
                alert("Error al crear la sala: " + error);
            });
        },
    
    eliminarSala: async function(){
      if(localStorage.getItem("token_access") === null || localStorage.getItem("token_refresh") === null){
                this.$emit("loadLogout");
                return;
            }

            localStorage.setItem("token_access", "");

            //promesa para validar le usuario logeado, eso creo
            await this.$apollo
                .mutate({
                mutation: gql`
                    mutation CreateSala($refresh: String!) {
                        refreshToken(refresh: $refresh) {
                            access
                        }
                    }`,
                    variables: {
                        refresh: localStorage.getItem("token_refresh"),
                    },
                })
                .then((result) => {
                    localStorage.setItem("token_access", result.data.refreshToken.access);
                })
                .catch((error) => {
                    this.$emit("loadLogout");
                    return;
                })
            //promesa para crear la sala
            await this.$apollo
                .mutate({
                    mutation: gql`
                        mutation DeleteSala($salaId: String!) {
                          deleteSala(salaId: $salaId)
                    }
                    `,
                    variables: {
                        salaId: this.salaId,
                    },
                })
                .then((result) => {
                    alert("Sala eliminada de manera correcta.");
                })
            .catch((error) => {
                alert("Apuesta eliminada: " + error + this.salaId);
                this.$router.push({ name: "transaction" })
            });
    },

        //metodo para pasar por medio de un props al componente de sala
    loadSala: function(id){
      console.log("id: " + id);
      this.$router.push({ name: "sala", params: { data_sala: id } });
    }
  }
}
</script>