<template>
<div >
  <div class="container py-6">
    <section class="seccion-perfil-usuario">
        <div class="perfil-usuario-header">
            <div class="perfil-usuario-portada">
                <div class="perfil-usuario-avatar">
                    <img src="../assets/mascota.png" alt="img-avatar" width="100" height="180">
                    <button type="button" class="boton-avatar" >
                        <i class="fa fa-thumbs-up"></i>
                    </button>
                </div>
                <button type="button" class="boton-portada" v-on:click="loadApuestas">
                    <i class="fa fa-flag"></i> Historial
                </button>
            </div>
        </div>
        <div class="perfil-usuario-body">
            <div class="perfil-usuario-bio">
                <h3 class="titulo">{{ username }}</h3>
                <p class="texto">
                    Dinero en Cuenta : ${{ accountByUsername.balance }} _________________________ Ultimo Movimiento =
                    {{ accountByUsername.lastChange.substring(0, 10) }}  
                    {{ accountByUsername.lastChange.substring(11, 19) }}
                  </p>
            </div>
            <div class="perfil-usuario-footer">
                <ul class="lista-datos">
                    <li><i class=""></i> Inversion: $ {{ accountByUsername.inversion}}</li>
                    <li><i class=""> </i> Activo: $ {{ accountByUsername.activo}}  </li>
                    <li><i class=""></i> Activo Neto: $ {{ accountByUsername.activoNeto}}</li>
                    <li><i class=""></i> Pasivo: $ {{ accountByUsername.pasivo}}</li>
                </ul>
                <ul class="lista-datos">
                    <li><i class=""></i> Ultima Apuesta: $ {{ accountByUsername.ultimaApuesta}}</li>
                    <li><i class=""></i> Apuestas Ganadas: {{ accountByUsername.apuestasGanadas}}</li>
                    <li><i class=""></i> Nivel: {{ accountByUsername.nivel}}</li>
                    <li><i class=""></i> Usuario: {{ username }}</li>
                </ul>
            </div>
            
        </div>
    </section>

    

  </div>
</div>
 

</template>


<script>
import gql from "graphql-tag";

export default {
  name: "Account",

  data: function () {
    return {
      username: localStorage.getItem("username") || "none",
      transactionByUsername: [],
      accountByUsername: {
        balance: "",
        lastChange: "",
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

  methods:{
    loadApuestas: function () {
        this.$router.push({ name: "apuestas" });
      },
  },
  created: function () {
    this.$apollo.queries.transactionByUsername.refetch();
    this.$apollo.queries.accountByUsername.refetch();
  }
};
</script>


<style type="text/css">
/*=====================================
reset estilos
no es necesario que copies esto
=====================================*/

html {
    -webkit-text-size-adjust: 100%;
    -ms-text-size-adjust: 100%;
    text-size-adjust: 100%;
    line-height: 1.4;
}


* {
    margin: 0;
    padding: 0;
    -webkit-box-sizing: border-box;
    -moz-box-sizing: border-box;
    box-sizing: border-box;
}

body {
    color: #404040;
    font-family: "Arial", Segoe UI, Tahoma, sans-serifl, Helvetica Neue, Helvetica;
}

/*=====================================
estilos de la utilidad
Copiar esto
=====================================*/
.seccion-perfil-usuario .perfil-usuario-body,
.seccion-perfil-usuario {
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    align-items: center;
}

.seccion-perfil-usuario .perfil-usuario-header {
    width: 100%;
    display: flex;
    justify-content: center;
    background: linear-gradient(#a5d4e9, transparent);
    margin-bottom: 1.25rem;
}

.seccion-perfil-usuario .perfil-usuario-portada {
    display: block;
    position: relative;
    width: 90%;
    height: 17rem;
    background-image: url('../assets/apubalon.jpg');    
    background-size: cover;
    border-radius: 0 0 20px 20px;
}

.seccion-perfil-usuario .perfil-usuario-portada .boton-portada {
    position: absolute;
    top: 1rem;
    right: 1rem;
    border: 0;
    border-radius: 8px;
    padding: 12px 25px;
    background-color: rgba(46, 183, 233, 0.686);
    color: #fff;
    cursor: pointer;
}

.seccion-perfil-usuario .perfil-usuario-portada .boton-portada i {
    margin-right: 1rem;
}

.seccion-perfil-usuario .perfil-usuario-avatar {
    display: flex;
    width: 180px;
    height: 180px;
    align-items: center;
    justify-content: center;
    border: 7px solid #FFFFFF;
    background-color: #DFE5F2;
    border-radius: 50%;
    box-shadow: 0 0 12px rgba(0, 0, 0, .2);
    position: absolute;
    bottom: -40px;
    left: calc(50% - 90px);
    z-index: 1;
}

.seccion-perfil-usuario .perfil-usuario-avatar img {
    width: 100%;
    position: relative;
    border-radius: 50%;
}

.seccion-perfil-usuario .perfil-usuario-avatar .boton-avatar {
    position: absolute;
    left: -2px;
    top: -2px;
    border: 0;
    background-color: #fff;
    box-shadow: 0 0 12px rgba(0, 0, 0, .2);
    width: 45px;
    height: 45px;
    border-radius: 50%;
    cursor: pointer;
}

.seccion-perfil-usuario .perfil-usuario-body {
    width: 70%;
    position: relative;
    max-width: 750px;
}

.seccion-perfil-usuario .perfil-usuario-body .titulo {
    display: block;
    width: 100%;
    font-size: 1.75em;
    margin-bottom: 0.5rem;
}

.seccion-perfil-usuario .perfil-usuario-body .texto {
    color: #0d79b3;
    font-size: 0.95em;
}

.seccion-perfil-usuario .perfil-usuario-footer,
.seccion-perfil-usuario .perfil-usuario-bio {
    display: flex;
    flex-wrap: wrap;
    padding: 1.5rem 2rem;
    box-shadow: 0 0 12px rgba(0, 0, 0, .2);
    background-color: #fff;
    border-radius: 15px;
    width: 100%;
}

.seccion-perfil-usuario .perfil-usuario-bio {
    margin-bottom: 1.25rem;
    text-align: center;
}

.seccion-perfil-usuario .lista-datos {
    width: 50%;
    list-style: none;
}

.seccion-perfil-usuario .lista-datos li {
    padding: 5px 0;
}

.seccion-perfil-usuario .lista-datos li>.icono {
    margin-right: 1rem;
    font-size: 1.2rem;
    vertical-align: middle;
}

.seccion-perfil-usuario .redes-sociales {
    position: absolute;
    right: calc(0px - 50px - 1rem);
    top: 0;
    display: flex;
    flex-direction: column;
}

.seccion-perfil-usuario .redes-sociales .boton-redes {
    border: 0;
    background-color: #fff;
    text-decoration: none;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    color: #fff;
    box-shadow: 0 0 12px rgba(0, 0, 0, .2);
    font-size: 1.3rem;
}

.seccion-perfil-usuario .redes-sociales .boton-redes+.boton-redes {
    margin-top: .5rem;
}

.seccion-perfil-usuario .boton-redes.facebook {
    background-color: #5955FF;
}

.seccion-perfil-usuario .boton-redes.twitter {
    background-color: #35E1BF;
}

.seccion-perfil-usuario .boton-redes.instagram {
    background: linear-gradient(45deg, #FF2DFD, #40A7FF);
}

/* adactacion a dispositivos */
@media (max-width: 750px) {
    .seccion-perfil-usuario .lista-datos {
        width: 100%;
    }

    .seccion-perfil-usuario .perfil-usuario-portada,
    .seccion-perfil-usuario .perfil-usuario-body {
        width: 95%;
    }

    .seccion-perfil-usuario .redes-sociales {
        position: relative;
        flex-direction: row;
        width: 100%;
        left: 0;
        text-align: center;
        margin-top: 1rem;
        margin-bottom: 1rem;
        align-items: center;
        justify-content: center
    }

    .seccion-perfil-usuario .redes-sociales .boton-redes+.boton-redes {
        margin-left: 1rem;
        margin-top: 0;
    }
}
</style>