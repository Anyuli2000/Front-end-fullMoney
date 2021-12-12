<template>
<div>
    <div class="container py-4">

        <nav class="navbar navbar-dark bg-primary">
            <div class="container-fluid">
                <span class="navbar-brand mb-0 h1">...</span>
                <span class="navbar-brand mb-0 h1">Historial de Apuestas</span>
                <span class="navbar-brand mb-0 h1">...</span>
            </div>
        </nav>
        <div class="table-responsive py-2">
                <table class="table table-light table-striped">
                    <thead>
                        <tr>                     
                            <th>Fecha</th>
                            <th>Hora</th>
                            <th>Origen</th>
                            <th>Destino</th>
                            <th>Valor</th>                  
                        </tr>
                    </thead>
                    <tbody>
                        <tr  v-for="transaction in transactionByUsername" :key="transaction.id">
                            <td>{{ transaction.date.substring(0, 10) }}</td>
                            <td>{{ transaction.date.substring(11, 19) }}</td>
                            <td>{{ transaction.cuentaOrigen }}</td>
                            <td>{{ transaction.cuentaDestino }}</td>
                            <td>${{ transaction.dinero }} COP</td>                 
                        </tr>
                    </tbody>
                </table>
        </div>
                               
        <div class="row row-cols-1 row-cols-md-2 g-4">
            <div class="col">
               
                <div class="card" >
                    <div class="row g-0">
                        <div class="col-md-4">
                        <img src="../assets/Tigresito.png" class="img-fluid rounded-start" alt="...">
                        </div>
                        <div class="col-md-8">
                        <div class="card-body">
                            <h5 class="card-title">Apuestas Perdidas</h5>
                            
                            <p class="card-text" v-for="apuestasPerdidas in accountByUsername.apuestasPerdidas" :key="apuestasPerdidas.username"> 
                                {{ apuestasPerdidas}} 
                            </p>
                            

                        </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col">
                <div class="card " >
                    <div class="row g-0">
                        <div class="col-md-4">
                        <img src="../assets/Leoncito.jpg" class="img-fluid rounded-start" alt="...">
                        </div>
                        <div class="col-md-8">
                        <div class="card-body">
                            <h5 class="card-title">Apuestas Ganadas</h5>
                            <p class="card-text" v-for="apuestasExitosas in accountByUsername.apuestasExitosas" :key="apuestasExitosas.username"> 
                                {{ apuestasExitosas}} 
                            </p>
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
        name: "Apuestas",
        data: function () {
            return {
            username: localStorage.getItem("username") || "none",
            transactionByUsername: [],
            accountByUsername: {
                balance: "",
                lastChange: "",
                apuestasExitosas: "",
                apuestasPerdidas: "",
                inversion: "",
                activo: "",
                activoNeto: "",
                pasivo: "",

                ultimaApuesta: "",
                apuestasGanadas: "",
                nivel: "",
            }
            };
        },

        apollo: {
            transactionByUsername: {
            query: gql`
                query ($username: String!) {
                transactionByUsername(username: $username) {
                    id
                    cuentaOrigen
                    cuentaDestino
                    dinero
                    equipo
                    date
                }
                }
            `,
            variables() {
                return {
                username: this.username,
                };
            },
            },

            accountByUsername: {
            query: gql`
                query ($username: String!) {
                accountByUsername(username: $username) {
                    balance
                    lastChange
                    apuestasExitosas
                    apuestasPerdidas
                    inversion
                    activo
                    activoNeto
                    pasivo
                    ultimaApuesta
                    apuestasGanadas
                    nivel
                }
                }
            `,
            variables() {
                return {
                username: this.username,
                };
            },
            },
        },

        
        created: function () {
            this.$apollo.queries.transactionByUsername.refetch();
            this.$apollo.queries.accountByUsername.refetch();
        }
    };
    
</script>