<template>

    <div id ='mi-componente'  class="container py-5 "  >
       
        
        <form class=" container py-8  col-sm-12 col-md-6 col-lg-4 col-xl-4 card" v-on:submit.prevent="processLogInUser">

            <h3>{{titulo}}</h3>
            <img src="../assets/logito.png" alt="">
            <div class="card-body">
                
                <div class="mb-3 " >
                    <label for="exampleInputEmail1" class="form-label">Username</label>
                    <input type="text" class="form-control" v-model="user.username" aria-describedby="emailHelp">
                    <div id="emailHelp" class="form-text">La seguridad de su cuenta es lo mas importante para fullMoney,Disfrute y confie.</div>
                </div>
                <div class="mb-3">
                    <label for="exampleInputPassword1" class="form-label">Contraseña</label>
                    <input type="password" v-model="user.password" class="form-control" >
                </div>
                <div class="mb-3 form-check">
                    <input type="checkbox" class="form-check-input" id="exampleCheck1">
                    <label class="form-check-label" for="exampleCheck1">Recuerdame</label>
                </div>
                <button type="submit" class="btn btn-primary">Iniciar Sesion</button>
            </div>
        </form>
    </div>

</template>


<script>
import gql from "graphql-tag";

export default {
  name: "LogIn",

  data: function() {
    return {
      user: {
        username: "",
        password: "",
      },
    };
  },

  methods: {
    processLogInUser: async function() {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation($credentials: CredentialsInput!) {
              logIn(credentials: $credentials) {
                refresh
                access
              }
            }
          `,
          variables: {
            credentials: this.user,
          },
        })
        .then((result) => {
          let dataLogIn = {
            username: this.user.username,
            token_access: result.data.logIn.access,
            token_refresh: result.data.logIn.refresh,
          };

          this.$emit("completedLogIn", dataLogIn);
        })
        .catch((error) => {
          alert("ERROR 401: Credenciales Incorrectas.");
        });
    },
  },
}
</script>


<style>


</style>