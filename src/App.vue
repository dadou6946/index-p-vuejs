<template>
  <div id="app" class="container">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/">About</router-link>
    </div>
    <router-view/>
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
    /*border: 1px solid #a3a3a3;*/
    border-radius: 4px;
    border: 1px solid rgba(0,0,0,0.2);
    color: #fff;
    font-size: 10px;
    font-weight: normal;
    line-height: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgb(0 0 0 / 70%);
    text-transform: uppercase;
    /*transition: opacity .4s;*/
}
</style>
