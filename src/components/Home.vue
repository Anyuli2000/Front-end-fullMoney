<template>

  <div class="information">
    <div class="container py-1">

      <div class="container py-2">
        <nav class="navbar navbar-expand-lg navbar-light bg-light">
          <div class="container-fluid">
            <img src="../assets/logito.png" alt="" width="80" height="38">
            <a class="navbar-brand" >Bienveni@ a Fullmoney  </a>
            <a class="navbar-brand" >{{ userDetailById.name }}</a>
          </div>
        </nav>
      </div>     

      <div class="row row-cols-1 row-cols-md-2 g-4 container py-4">
        <div class="col">
          <div class="card">
            <img src="../assets/messi.jpg" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Polemica sobre el balon de oro de Leo</h5>
              <p class="card-text">               
                Un récord que bien vale un Balón de Oro
                El gran favorito. En las horas previas ya se vislumbraba su triunfo
                Lionel Messi ha ganado el séptimo Balón de Oro de su carrera deportiva esta noche en París, donde la FIFA y France Football le han reconocido ante los asistentes del Theatre du Chatelet como el mejor jugador del mundo en el año 2021...
              </p>
            </div>
          </div>
        </div>
        <div class="col">
          <div class="card">
            <img src="../assets/diaz.jpg" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Luis Diaz en la mira del Liverpool</h5>
              <p class="card-text">
                Luis Díaz es un talento puro, eso no es un secreto, y por ese motivo ya comenzó a llamar la atención de clubes gigantes en Europa, como al que enfrentó este miércoles: Liverpool.
                Hace algunas semanas se rumorea que Jurgen Klopp lo tiene en la mira para tener más variantes en el ataque y, al parecer, pudo convencerse al verlo tan cerca, en el duelo de Champions League.             
              </p>
            </div>
          </div>
        </div>
        <div class="col">
          <div class="card">
            <img src="../assets/cristiano.jpg" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Cristiano en el psg?</h5>
              <p class="card-text">
                Ante la presión de Mbappé para irse al Real Madrid, los dueños del PSG facilitarían su salida en venta ya, pues saben que el próximo año quedará libre y no quieren dejar de ganar una buena cantidad de dinero por la perla francesa.
              Si se va Mbappé se abre espacio para Cristiano, quien incluso heredaría la camiseta #7 y el PSG se mantendría como el club con más figuras de toda Europa
              </p>
            </div>
          </div>
        </div>
        <div class="col">
          <div class="card">
            <img src="../assets/cuadrado.jpg" class="card-img-top" alt="...">
            <div class="card-body">
              <h5 class="card-title">Gooool olimpico de Cuadrado</h5>
              <p class="card-text">
                "Es el mejor gol de mi carrera", así definió Juan Guillermo Cuadrado su anotación ante Genoa que sirvió para conseguir una importante victoria en Serie A que por ahora los tiene en el quinto puesto de la tabla con 27 unidades. El equipo de Allegri poco a poco va encontrando el camino a la regularidad y el colombiano demuestra ser fundamental para contribuir a la reacción del equipo que tuvo un arranque complicado en la liga. 
              </p>
            </div>
          </div>
        </div>
      </div>


    </div>
     
  </div>
  
</template>


<script>
import gql from "graphql-tag";
import jwt_decode from "jwt-decode";

export default {
  name: "Home",

  data: function () {
    return {
      userId: jwt_decode(localStorage.getItem("token_refresh")).user_id,
      userDetailById: {
        username: "",
        name: "",
        email: "",
      },
    };
  },

  apollo: {
    userDetailById: {
      query: gql`
        query ($userId: Int!) {
          userDetailById(userId: $userId) {
            username
            name
            email
          }
        }
      `,
      variables() {
        return {
          userId: this.userId,
        };
      }
    },
  }
};
</script>


<style>




</style>
