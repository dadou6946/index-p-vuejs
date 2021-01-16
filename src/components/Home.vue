<template>
  <div class="home">

    <div class="row">
      <h1>home</h1>
    </div>
    
    <div class="row">
      <div class="col s3 m3" 
        v-for="(pkmn, index) in pkmnData" 
        :key="index+1">
        <div class="card">
          <div class="card-content">
            <span class="card-title">{{ pkmn.name | capitalize }}</span>
          </div>
          <div class="card-action">
            <router-link :to="'/detail/'+(index+1)"
              class="waves-effect indigo lighten-1 white-text waves-light btn">
              More
            </router-link>
          </div>
        </div>
      </div>
    </div>

    <div class="row">
      <a @click="changeList('previous')" 
        class="btn indigo lighten-1 white-text"
        :disabled="urlPrevious==null">
        Previous
      </a>
      <span> {{ idLow }} - {{ idHight }} </span>
      <a @click="changeList('next')" 
        class="btn indigo lighten-1 white-text"
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
    }
  },
  methods : {
    changeList(type) {
      console.log(type)
      if (type == 'next')
        this.urlCurrent = this.urlNext
      else
        this.urlCurrent = this.urlPrevious
      axios
      .get(this.urlCurrent)
      .then(response => {
        console.log(response.data)
        this.pkmnData = response.data.results
        this.urlPrevious = response.data.previous
        this.urlNext = response.data.next
        this.idLow+=30
        this.idHight+=30
      })
    }
  },
  mounted () {
    this.urlCurrent = 'https://pokeapi.co/api/v2/pokemon?limit=30',
    this.idLow = 1
    this.idHight = 30
    axios
      .get(this.urlCurrent)
      .then(response => {
        this.pkmnData = response.data.results
        this.urlPrevious = response.data.previous
        this.urlNext = response.data.next
        console.log(response.data)
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
</style>