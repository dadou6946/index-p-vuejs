<template>
  <div class="home">

    <div class="row">
      <h1>Pokedex</h1>
    </div>
    
    <!-- affichage de tuile pour chaque pkmn -->
    <div class="row">
      <div class="col s3 m3" 
        v-for="(pkmn, index) in pkmnData" 
        :key="index+1">
        <div class="card">
          <router-link :to="'/detail/'+(index+indexChange+1)">
            <img class="pkmn-picture" 
              :src="pkmn.picture" 
              :alt="pkmn.name">
          </router-link>
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
</template>

<script>

import axios from 'axios';

export default {
  name: 'Home',
  data () {
    return {
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
        // mise a jour des données, url et index
        this.pkmnData = response.data.results
        this.urlPrevious = response.data.previous
        this.urlNext = response.data.next
        this.idLow+=30
        this.idHight+=30
        // recherche des photo avec les nouveaux enregistrements
        this.getPictures()
      })
    },
    // Recherche des photos pour chaque enregistrement de pkmn dans pkmndata
    getPictures() {
      this.pkmnData.forEach((pkmnd, index) => {
        axios
          .get(pkmnd.url)
          .then(response => {
            this.$set(this.pkmnData[index], 'picture', response.data.sprites.front_default)
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
</style>