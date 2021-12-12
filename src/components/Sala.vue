<template>
<div >       
    <div class="container py-4">

        <nav class="navbar navbar-dark bg-primary">
            <div class="container-fluid">
                <span class="navbar-brand mb-0 h1">{{ salaById.cuenta }}</span>
                <span class="navbar-brand mb-0 h1">{{ data_sala }}</span>
                <span class="navbar-brand mb-0 h1">{{ salaById.tarifa }}</span>
            </div>
        </nav>

            
        <div class="container py-2">
        <div class="row">


            <div  class = " col-sm-12 col-md-6 col-lg-4 col-xl-4 " >
                
                <div class="card">
                    <img src="../assets/apos.jpg" alt="">
                    <div class="card-body">
                     
                     <div class="mb-3">
                     <h5 class="card-title">   Apuesta a tu equipo favorito</h5>
                     </div>
                    
                    <form v-on:submit.prevent="procesoTransaccion">
                            
                        <div class="mb-3">             
                           <input class="form-control" type="text" v-model="transaction.id" placeholder="Id de la transaccion" required>              
                        </div>
                        <div class="mb-3">             
                            <input  class="form-control" type="text" v-model="transaction.cuentaOrigen" placeholder="cuenta origen" required>
                        </div>
                        <div class="mb-3 ">              
                            <input class="form-control" type="text" v-model="transaction.cuentaDestino" placeholder="cuenta destino">
                        </div>
                        <div class="mb-3 ">              
                            <input class="form-control" type="number" v-model="transaction.dinero" placeholder="dinero" required>
                        </div>
                        <div class="mb-3 ">              
                            <input class="form-control" type="text" v-model="transaction.equipo" placeholder="Equipo" required>
                        </div>
                        <button type="submit" class="btn btn-success">Apostar</button>
                    </form>
                    </div>
                </div>
            </div>

            <div  class = " col-sm-12 col-md-6 col-lg-8 col-xl-8 " >

                <nav class="navbar navbar-light bg-light">
                    <div class="container-fluid">

                
                        <!-- Button trigger modal -->
                        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal1">
                            Marcador
                        </button>

                        <!-- Modal -->
                        <div class="modal fade" id="exampleModal1" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                            <div class="modal-dialog modal-dialog-centered">
                                <div class="modal-content">
                                    <div class="modal-header">
                                        <h5 class="modal-title" id="exampleModalLabel">Editar Marcador</h5>
                                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                                    </div>
                                    <div class="modal-body">
                                        <!-- metodo para editar -->
                                        <form v-on:submit.prevent="editarSala">
                                            <div class="mb-2 ">
                                                <input class="form-control" type="text"  placeholder="Codigo Sala" v-model="salaId">
                                            </div>
                                            <div class="mb-2 ">
                                                <input class="form-control" type="number"  placeholder="Marcador 1" v-model="sala.marcador1">
                                            </div>
                                            <div class="mb-2 ">
                                                <input class="form-control" type="number"  placeholder="Marcador 2" v-model="sala.marcador2">
                                            </div>
                                            <div class="mb-2 ">
                                                <button type="submit" class="btn btn-primary">Actualizar</button>
                                            </div>
                                        </form>
                                    </div>
                                    <div class="modal-footer">
                                        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                                        
                                    </div>
                                </div>
                            </div>
                        </div>        

                        <span class="navbar-brand mb-0 h1">{{ salaById.titulo }}</span>
                        <form v-on:submit.prevent="finalizarSala">
                            <button type="submit" class="btn btn-primary">Finalizar</button>
                        </form>
                    </div>
                </nav>

                <iframe width="722" height="450" 
                v-bind:src="`${salaById.imagen}`"
                title="YouTube video player" frameborder="0" 
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
                allowfullscreen></iframe>

                <nav class="navbar navbar-light bg-light">
                    <div class="container-fluid">
                        
                        <!-- Button trigger modal -->
                        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal2">
                        Apuestas por {{ salaById.equipo1 }}
                        </button>

                        <!-- Modal -->
                        <div class="modal fade" id="exampleModal2" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                            <div class="modal-dialog">
                                <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="exampleModalLabel">Usuarios * {{ salaById.equipo1 }}</h5>
                                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                                </div>
                                <div class="modal-body">
                                    <p v-for="equipo1 in salaById.equipoA" :key="equipo1.id">
                                        {{ equipo1 }}
                                    </p>
                                </div>
                                <div class="modal-footer">
                                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>                                
                                </div>
                                </div>
                            </div>
                        </div>

                        <span class="navbar-brand mb-0 h1">{{ salaById.marcador1 }} - {{ salaById.marcador2 }}</span>
                        
                        <!-- Button trigger modal -->
                        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal3">
                        Apuestas por {{ salaById.equipo2 }}
                        </button>

                        <!-- Modal -->
                        <div class="modal fade" id="exampleModal3" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
                            <div class="modal-dialog">
                                <div class="modal-content">
                                <div class="modal-header">
                                    <h5 class="modal-title" id="exampleModalLabel">Usuarios * {{ salaById.equipo2 }}</h5>
                                    
                                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                                </div>
                                <div class="modal-body">
                                    <p v-for="equipo2 in salaById.equipoB" :key="equipo2.id">
                                        {{ equipo2 }}
                                    </p>
                                </div>
                                <div class="modal-footer">
                                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>                                
                                </div>
                                </div>
                            </div>
                        </div>

                    </div>
                </nav>

            </div>    

        </div>
        </div>
    </div>
</div>





</template>

<script>
import gql from "graphql-tag";

export default {
    name: "Sala",

    data: function(){
        return {
            transaction: {
                "id": "",
                "cuentaOrigen": localStorage.getItem("username"),
                "cuentaDestino": "",
                "dinero": 0,
                "equipo": ""
            },
            
            sala: {
                "marcador1": 0,
                "marcador2": 0
                },
            salaId: "",

            salaId: ""
        }
    },

    //para crear transaccion y editar sala
    methods: {
        procesoTransaccion: async function() {

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
            //promesa para crear transaccion
            await this.$apollo
                .mutate({
                    mutation: gql`
                       mutation Mutation($transaction: TransactionInput!) {
                        createTransaction(transaction: $transaction) {
                            id
                            cuentaOrigen
                            cuentaDestino
                            dinero
                            equipo
                            date
                        }
                    }
                    `,
                    variables: {
                        transaction: this.transaction,
                    },
                })
                .then((result) => {
                    alert("Transaccion creada de manera exitosa.");
                    //this.$router.push({name: "transaction" })
                })
            .catch((error) => {
                alert("Error al crear la transaccion: " + error + this.transaction.id + "\n" + this.transaction.cuentaOrigen + "\n" + this.transaction.cuentaDestino + "\n" + this.transaction.dinero + "\n" + this.transaction.equipo);
            });
        },

        editarSala: async function() {

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
            //promesa para crear transaccion
            await this.$apollo
                .mutate({
                    mutation: gql`
                    mutation EditarSala($salaId: String!, $sala: EditarSala!) {
                        editarSala(salaId: $salaId, sala: $sala) {
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
                        salaId: this.salaId
                    },
                })
                .then((result) => {
                    alert("El marcador se editó bien.");
                })
            .catch((error) => {
                alert("Error al editar el marcador de la sala: " + error + this.sala.marcador1 + "\n" + this.sala.marcador2 + "\n" + this.salaId);
            });
        },

        finalizarSala: async function(){
            alert("finalizando sala...");
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
            //promesa para crear transaccion
            await this.$apollo
                .mutate({
                    mutation: gql`
                       mutation TerminarSala($salaId: String!) {
                        terminarSala(salaId: $salaId)
                    }
                    `,
                    variables: {
                        salaId: this.data_sala,
                    },
                })
                .then((result) => {
                    alert("Sala finalizada.");
                })
            .catch((error) => {
                alert("Error al finalizar la sala: " + error + this.salaId);
            });
        },
    },

    props:{
        data_sala: {
            id: {
                type: String
            }
        }
    },
    created(){
        console.log("Data traida desde el componente Transaction: " + this.data_sala);
    },
    
    apollo: {
        salaById: {
            query: gql`
                query Query($salaId: String!) {
                salaById(salaId: $salaId) {
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
            variables() {
            return {
                salaId: this.data_sala
            };
        }
        }
    }

}
</script>