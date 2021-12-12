<template>

  <div id="app" class="app">

    
     <div class="area"></div><nav class="main-menu">
            <ul>
                <li>
                    <a v-if="is_auth" v-on:click="loadHome">
                        <i class="fa fa-home fa-2x"></i>
                        <span class="nav-text">
                          inicio
                        </span>
                    </a>
                  
                </li>
                <li class="has-subnav">
                    <a v-if="is_auth" v-on:click="loadAccount">
                        <i class="fa fa-th-large"></i>
                        <span class="nav-text">
                            Cuenta
                        </span>
                    </a>
                    
                </li>
                <li class="has-subnav">
                    <a v-if="is_auth" v-on:click="loadTransaction">
                       <i class="fa fa-money"></i>
                        <span class="nav-text">
                            Salas * Apostar
                        </span>
                    </a>
                    
                </li>
                <li class="has-subnav">
                    <a v-if="!is_auth" v-on:click="loadLogIn">
                       <i class="fa fa-user fa-fw"></i>
                        <span class="nav-text">
                            Iniciar Sesion
                        </span>
                    </a>
                   
                </li>
                <li>
                    <a v-if="!is_auth" v-on:click="loadSignUp">
                        <i class="fa fa-book fa-fw"></i>
                        <span class="nav-text">
                            Registrarse
                        </span>
                    </a>
                </li>                                          
            </ul>

            <ul class="logout">
                <li>
                   <a v-if="is_auth" v-on:click="logOut">
                         <i class="fa fa-power-off fa-2x"></i>
                        <span class="nav-text">
                            Logout
                        </span>
                    </a>
                </li>  
            </ul>
        </nav>


    <div class="main-component">
      <router-view  
        v-on:completedLogIn="completedLogIn"
        v-on:completedSignUp="completedSignUp"
        v-on:logOut="logOut"
      >
      </router-view>
    </div>
    


  </div>

</template>


<script>
export default {
  name: 'App',

  computed: {
    is_auth: {
      get: function() {
        return this.$route.meta.requiresAuth;
      },
      set: function() { }
    }
  },

  methods:{

    loadLogIn: function(){
      this.$router.push({name: "logIn"})
    },

    loadSignUp: function(){
      this.$router.push({name: "signUp"})
    },

    completedLogIn: function(data) {
			localStorage.setItem("username", data.username);
			localStorage.setItem("token_access", data.token_access);
			localStorage.setItem("token_refresh", data.token_refresh);
			alert("Autenticación Exitosa");
			this.loadHome();
    },

    completedSignUp: function(data) {
			alert("Registro Exitoso");
			this.completedLogIn(data);
    },

    loadHome: function() {
      this.$router.push({ name: "home" });
    },

    loadAccount: function () {
			this.$router.push({ name: "account" });
		},

    loadTransaction: function(){
      this.$router.push({ name: "transaction" });
    },

    logOut: function () {
			localStorage.clear();
			alert("Sesión Cerrada");
      this.loadLogIn();
		}
  }
}
</script>


<style>

  *{
    margin:0;
    padding: 0;
    box-sizing: border-box;
  }

  #app{
    background-image: url('./assets/212835.jpg');
    background-attachment: fixed;
    background-size: cover; 
    background-position: 5% center;
    min-height: 100vh;
    ;
  }

  main {
    min-height: 100vh;
  }

@import url(//netdna.bootstrapcdn.com/font-awesome/4.0.3/css/font-awesome.css);

@import url(https://fonts.googleapis.com/css?family=Titillium+Web:300);
.fa-2x {
  font-size: 2em;
}
.fa {
  position: relative;
  display: table-cell;
  width: 60px;
  height: 36px;
  text-align: center;
  vertical-align: middle;
  font-size:20px;
}


.main-menu:hover,nav.main-menu.expanded {
  width:250px;
  overflow:visible;
}

.main-menu {
  background:#fdfdf9fd;
  border-right:1px solid #fff;
  position:absolute;
  top:0;
  bottom:0;
  height:100%;
  left:0;
  width:60px;
  overflow:hidden;
  -webkit-transition:width .05s linear;
  transition:width .05s linear;
  /*-webkit-transform:translateZ(0) scale(1,1);*/
  z-index:1000;
}

.main-menu>ul {
  margin:7px 0;
}

.main-menu li {
position:relative;
display:block;
width:250px;
}

.main-menu li>a {
  position:relative;
  display:table;
  border-collapse:collapse;
  border-spacing:0;
  color:#999;
  font-family: arial;
  font-size: 14px;
  text-decoration:none;
  /*-webkit-transform:translateZ(0) scale(1,1);*/
  -webkit-transition:all .1s linear;
  transition:all .1s linear;
  
}

.main-menu .nav-icon {
  position:relative;
  display:table-cell;
  width:60px;
  height:36px;
  text-align:center;
  vertical-align:middle;
  font-size:18px;
}

.main-menu .nav-text {
  position:relative;
  display:table-cell;
  vertical-align:middle;
  width:190px;
    font-family: 'Titillium Web', sans-serif;
}

.main-menu>ul.logout {
  position:absolute;
  left:0;
  bottom:0;
}

.no-touch .scrollable.hover {
  overflow-y:hidden;
}

.no-touch .scrollable.hover:hover {
  overflow-y:auto;
  overflow:visible;
}

a:hover,a:focus {
  text-decoration:none;
}

nav {
  -webkit-user-select:none;
  -moz-user-select:none;
  -ms-user-select:none;
  -o-user-select:none;
  user-select:none;
}

nav ul,nav li {
  outline:0;
  margin:0;
  padding:0;
}
.main-menu li:hover>a,nav.main-menu li.active>a,.dropdown-menu>li>a:hover,.dropdown-menu>li>a:focus,.dropdown-menu>.active>a,.dropdown-menu>.active>a:hover,.dropdown-menu>.active>a:focus,.no-touch .dashboard-page nav.dashboard-menu ul li:hover a,.dashboard-page nav.dashboard-menu ul li.active a {
  color:#fff;
  background-color:#5fa2db;
}
.area {
  float: left;
  background: #e2e2e2;
  width: 100%;
  height: 100%;
}
@font-face {
  font-family: 'Titillium Web';
  font-style: normal;
  font-weight: 300;
  src: local('Titillium WebLight'), local('TitilliumWeb-Light'), url(http://themes.googleusercontent.com/static/fonts/titilliumweb/v2/anMUvcNT0H1YN4FII8wpr24bNCNEoFTpS2BTjF6FB5E.woff) format('woff');
}

</style>
