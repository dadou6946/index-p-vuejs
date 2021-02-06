<template>
  <div id="app">

    <!-- Header -->
    <div class="container-fluid">
      <nav class="red">
        <div class="nav-wrapper">
          <router-link to="/" class="brand-logo right">lien de droite</router-link>
          <ul class="left hide-on-med-and-down">
            <li><router-link to="/"><img class="img-logo" :src="'/pokeball.png'"></router-link></li>
            <li><router-link to="/"><span>Home</span></router-link></li>
            <li><router-link to="">lien</router-link></li>
            <li class="active"><router-link to="">lien2</router-link></li>
          </ul>
        </div>
      </nav>
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
.type-normal {
    background-color: #aa9;
}
.type-fire {
    background-color: #f42;
}
.type-water {
    background-color: #39f;
}
.type-electric {
    background-color: #fc3;
}
.type-grass {
    background-color: #7c5;
}
.type-ice {
    background-color: #6cf;
}
.type-fighting {
    background-color: #b54;
}
.type-poison {
    background-color: #a59;
}
.type-ground {
    background-color: #db5;
}
.type-flying {
    background-color: #89f;
}
.type-psychic {
    background-color: #f59;
}
.type-bug {
    background-color: #ab2;
}
.type-rock {
    background-color: #ba6;
}
.type-ghost {
    background-color: #66b;
}
.type-dragon {
    background-color: #76e;
}
.type-dark {
    background-color: #754;
}
.type-steel {
    background-color: #aab;
}
.type-fairy {
    background-color: #e9e;
}

div.type-icon {
    display: inline-block;
    width: 66px;
    margin-bottom: 4px;
    border-radius: 4px;
    border: 1px solid rgba(0,0,0,0.2);
    color: #fff;
    font-size: 10px;
    font-weight: normal;
    line-height: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgb(0 0 0 / 70%);
    text-transform: uppercase;
}
</style>
