<template>
  <div id="app">

    <!-- Headers -->
    <div class="container-fluid">
      <nav class="red" v-bind:class="showExtendedHeader">

        <div class="nav-wrapper">
          <ul class="left hide-on-med-and-down">
            <li><router-link to="/"><img class="img-logo" :src="'/pokeball.png'"></router-link></li>
            <li v-bind:class="{ 'active' : showMenuGen}"><a @click="showMenu('generations')">Generations</a></li>
            <li v-bind:class="{ 'active' : showMenuTypes}"><a @click="showMenu('types')">By type</a></li>
          </ul>
        </div>

        <transition name="fade">
          <div class="nav-content" v-show="getVisibilty">
            <ul class="tabs tabs-transparent" v-show="showMenuGen">
              <li class="tab"><a class="active" href="#test1">Kanto</a></li>
              <li class="tab"><a>Johto</a></li>
              <li class="tab"><a>Hoenn</a></li>
              <li class="tab"><a>Sinnoh</a></li>
              <li class="tab"><a>Unova</a></li>
              <li class="tab"><a>Kalos</a></li>
              <li class="tab"><a>Alola</a></li>
              <li class="tab"><a>Galar</a></li>
            </ul>
            <ul class="tabs tabs-transparent" v-show="showMenuTypes">
              <li class="tab"><a>Normal</a></li>
              <li class="tab"><a>Grass</a></li>
              <li class="tab"><a>Fire</a></li>
              <li class="tab"><a>Water</a></li>
              <li class="tab"><a>Electric</a></li>
            </ul>
          </div>
        </transition>

      </nav>
      <!-- header mobile -->
      <!-- <ul class="sidenav" id="mobile-demo">
        <li><a href="sass.html">Sass</a></li>
        <li><a href="badges.html">Components</a></li>
        <li><a href="collapsible.html">JavaScript</a></li>
      </ul>
      <div id="test1" class="col s12">Test 1</div>
      <div id="test2" class="col s12">Test 2</div>
      <div id="test3" class="col s12">Test 3</div>
      <div id="test4" class="col s12">Test 4</div> -->
    </div>

    <!-- contenu -->
    <div class="container main-application-container" style="min-height: 900px;">
      <router-view/>
    </div>

    <!-- Footer -->
    <div class="container-fluid">
      <footer class="page-footer grey darken-1">
          <div class="container">
            <div class="row">
              <div class="col s12">
              <!-- <div class="col l6 s12"> -->
                <h5 class="white-text">Footer Content</h5>
                <p class="grey-text text-lighten-4">You can use rows and columns here to organize your footer content.</p>
              </div>
              <!-- <div class="col l4 offset-l2 s12">
                <h5 class="white-text">Links</h5>
                <ul>
                  <li><a class="grey-text text-lighten-3" href="#!">Link 1</a></li>
                  <li><a class="grey-text text-lighten-3" href="#!">Link 2</a></li>
                  <li><a class="grey-text text-lighten-3" href="#!">Link 3</a></li>
                  <li><a class="grey-text text-lighten-3" href="#!">Link 4</a></li>
                </ul>
              </div> -->
            </div>
          </div>
          <div class="footer-copyright">
            <div class="container">
            © 2021 Copyright 
            <!-- <a class="grey-text text-lighten-4 right" href="#!">More Links</a> -->
            </div>
          </div>
        </footer>
    </div>

  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      showMenuGen: false,
      showMenuTypes: false,
    }
  },
  computed: {
    showExtendedHeader(){
      if(this.showMenuTypes == true || this.showMenuGen == true)
        return 'nav-extended'
      else
        return ''
    },
    getVisibilty(){
      if(this.showMenuTypes == true)
        return true
      if( this.showMenuGen == true)
        return true
      return false
    },
  },
  methods : {
    // Méthode pour afficher les menus
    showMenu(type){
      if(type == 'types')
      {
        this.showMenuGen = false
        this.showMenuTypes = !this.showMenuTypes
      }
      else if(type == 'generations')
      {
        this.showMenuTypes = false
        this.showMenuGen = !this.showMenuGen
      }
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}

/*polices*/
div#app {
  font-family: 'Raleway', sans-serif;
}

h1,h2,h3 {
  /*font-family: 'Kaushan Script', cursive;*/
  font-family: 'Antic Slab', serif;
}

img.img-logo {
  height: 64px;
}

/*espace entre header et contenu*/
div.main-application-container {
  margin-top: 60px;
}

/*classes de couleurs - types*/
.type-normal { background-color: #aa9; }
.type-fire { background-color: #f42; }
.type-water { background-color: #39f; }
.type-electric { background-color: #fc3; }
.type-grass { background-color: #7c5; }
.type-ice { background-color: #6cf; }/**/
.type-fighting { background-color: #b54; }
.type-poison { background-color: #a59; }
.type-ground { background-color: #db5; }/**/
.type-flying { background-color: #89f; }
.type-psychic { background-color: #f59; }
.type-bug { background-color: #ab2; }
.type-rock { background-color: #ba6; }/**/
.type-ghost { background-color: #66b; }/**/
.type-dragon { background-color: #76e; }
.type-dark { background-color: #754; }/**/
.type-steel { background-color: #aab; }/**/
.type-fairy { background-color: #e9e; }

#loader {
  -webkit-animation: rotation 2s infinite linear;
}
@-webkit-keyframes rotation {
    from {
        -webkit-transform: rotate(0deg);
    }
    to {
        -webkit-transform: rotate(359deg);
    }
}

/*Animations*/
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
