<template>

    <div >
        <div class="container py-5 ">
            <form   v-on:submit.prevent="processSignUp" class=" container py-4  col-sm-12 col-md-6 col-lg-4 col-xl-4 card">
              
              <img src="../assets/Registrarse.png" alt="">
              
              <div class="mb-3">             
                <input  class="form-control"  type="text" v-model="user.username" placeholder="Usuario">               
              </div>
              <div class="mb-3">             
                <input type="password" class="form-control" v-model="user.password" placeholder="Contraseña">
              </div>
              <div class="mb-3 ">              
                <input class="form-control" type="text" v-model="user.name" placeholder="Nombre">
              </div>
              <div class="mb-3 ">              
                <input class="form-control" type="email" v-model="user.email" placeholder="Correo">
              </div>
              <div class="mb-3 ">              
                <input class="form-control" type="number" v-model="user.balance" placeholder="Saldo Inicial">
              </div>
              
              <button type="submit" class="btn btn-primary">Registrarme</button>
            </form>
        </div>

    </div>

</template>


<script>
import gql from "graphql-tag";

export default {
    name: "SignUp",

    data: function() {
        return {
        user: {
            username: "",
            password: "",
            name: "",
            email: "",
            balance: 0,
            apuestasExitosas: [],
            apuestasPerdidas: [],
            inversion: 0,
            activo: 0,
            activoNeto: 0,
            pasivo: 0,
            ultimaApuesta: 0,
            apuestasGanadas: 0,
            nivel: "",

        },
        };
    },

  methods: {
    processSignUp: async function() {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation($userInput: SignUpInput!) {
              signUpUser(userInput: $userInput) {
                refresh
                access
              }
            }
          `,
          variables: {
            userInput: this.user,
          },
        })
        .then((result) => {
          let dataLogIn = {
            username: this.user.username,
            token_access: result.data.signUpUser.access,
            token_refresh: result.data.signUpUser.refresh,
          };

          this.$emit("completedSignUp", dataLogIn);
        })
        .catch((error) => {
          alert("ERROR: Fallo en el registro.");
        });
    },

  },
}
</script>


<style>

    

</style>