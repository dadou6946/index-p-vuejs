<template>
  <div class="home">

    <transition name="fade">
      <div class="loader-container" v-if=showLoader>
        <img class="img-loader" 
          id="loader" 
          :src="'/loader.png'">
      </div>
    </transition>


    <div class="row">
      <h1>Pokedex</h1>
    </div>

    <transition name="fade">
      <div v-if="!showLoader">
        
        <!-- affichage de tuile pour chaque pkmn -->
        <div class="row">
          <div class="col s3 m2" 
            v-for="(pkmn, index) in pkmnData" 
            :key="index+1">
            <div class="card" v-if="pkmn.picture">
              <router-link :to="'/detail/'+(index+indexChange+1)">
                <img class="pkmn-picture"
                  :src="pkmn.picture" 
                  :alt="pkmn.name">
              </router-link>
              <div>
                <span v-for="type in pkmn.types"
                  :key="type.name"
                  class="type-icon" 
                  :class="'type-'+type.name">
                  {{ type.name | capitalize }}
                </span>
              </div>
              <div class="card-action">
                <router-link :to="'/detail/'+(index+indexChange+1)"
                  class="waves-effect red white-text waves-light btn">
                  {{ pkmn.name | capitalize }}
                </router-link>
              </div>
            </div>
          </div>
        </div>

        <!-- boutons enregistrements suivants -->
        <div class="row">
          <a @click="changeList('previous')" 
            class="btn red white-text"
            :disabled="urlPrevious==null">
            Previous
          </a>
          <span class="index-count">{{ idLow }} - {{ idHight }}</span>
          <a @click="changeList('next')" 
            class="btn red white-text"
            :disabled="urlNext==null">
            Next
          </a>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'Home',
  data () {
    return {
      showLoader: true,
      urlCurrent: '',
      urlPrevious: null,
      urlNext: null,
      pkmnData: [],
      idLow: 0,
      idHight: 0,
      indexChange: 0,
    }
  },
  methods : {
    // Méthode pour aller sur les enregistrements suivants/précédents
    changeList(type) {
      // On revient en haut de la page
      window.scrollTo({
        top:0,
        left: 0,
       behavior: 'smooth'
      });
      if (type == 'next')
      {
        this.urlCurrent = this.urlNext
        this.indexChange+= 30 
      }
      else
      {
        this.urlCurrent = this.urlPrevious
        this.indexChange-= 30 
      }
      axios
      .get(this.urlCurrent)
      .then(response => {

        this.showLoader = true
        // mise a jour des données, url et index
        this.pkmnData = response.data.results
        this.urlPrevious = response.data.previous
        this.urlNext = response.data.next
        this.idLow+=30
        this.idHight+=30
        // recherche des photo avec les nouveaux enregistrements
        this.getPictures()
      })
      // Affichage du loader 
      setTimeout(function () { this.showLoader=false }.bind(this), 1000)
    },
    // Recherche des photos pour chaque enregistrement de pkmn dans pkmndata
    getPictures() {
      this.pkmnData.forEach((pkmnd, index) => {
        axios
          .get(pkmnd.url)
          .then(response => {
            console.log(response.data)
            this.$set(this.pkmnData[index], 'picture', response.data.sprites.front_default)
            var types = []
            response.data.types.forEach((typeData) => {
              types.push(typeData.type)
            })
            this.$set(this.pkmnData[index], 'types', types)
          })
      })
    }
  },
  mounted () {
    this.urlCurrent = 'https://pokeapi.co/api/v2/pokemon?limit=30',
    this.idLow = 1
    this.idHight = 30
    // Recherche de la liste des pkmn
    axios
      .get(this.urlCurrent)
      .then(response => {
        this.pkmnData = response.data.results
        this.urlPrevious = response.data.previous
        this.urlNext = response.data.next
        // Recherche des photos
        this.getPictures()
      })
    setTimeout(function () { this.showLoader=false }.bind(this), 1000)

  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

/*image de pkmn*/
img.pkmn-picture {
  height: 90px;
  width: 90px;
  margin-top: 10px;
}

/*compte en bas des listes*/
span.index-count {
  margin: 15px;
}

/*loader*/
div.home {
  position: relative;
}
div.loader-container {
  position: absolute;
  width: 100%;
}
img.img-loader {
  z-index: 10;
  display: block;
  margin-left: auto;
  margin-right: auto;
  margin-top: 100px;
}

span.type-icon {
    display: inline-block;
    margin-bottom: 4px;
    border-radius: 4px;
    border: 1px solid rgba(0,0,0,0.2);
    color: #fff;
    font-size: 8px;
    font-weight: normal;
    line-height: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgb(0 0 0 / 70%);
    text-transform: uppercase;
    padding-right: 5px;
    padding-left: 5px;
}
</style>