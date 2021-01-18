<template>
  <div class="home" v-if="pkmnData !=null">

    <div class="row">

      <div class="col m6">
        <h4 id="pkmn-id">#{{ pkmnData.id }}</h4>
        <h3 id="pkmn-name">{{ pkmnData.name | capitalize }}</h3>
        <p id="pkmn-height">Height : {{ pkmnData.height*10 }} cm</p>
        <p id="pkmn-weight">Weight : {{ pkmnData.weight*0.1 }} kg</p>
        <div id="pkmn-types">
          <h5>Types :</h5>
          <span v-for="type in pkmnData.types"
            :key="type.slot"
            :class="'type-'+type.type.name">
            {{ type.type.name | capitalize }}
          </span>
        </div>
        <div id="pkmn-texts">
          <h5>Pokedex texts :</h5>
        </div>
        <div id="pkmn-abilities">
          <h5>Abilities :</h5>
        </div>
        <div id="pkmn-attacks">
          <h5>Attacks :</h5>
        </div>
      </div>

      <div class="col m6">
        <div class="row" id="pkmn-image-container">
          <img class="pkmn-image" :src="pkmnData.sprites.other['official-artwork'].front_default"
            style="height: 150px;" alt="">
          <img class="pkmn-image" :src="pkmnData.sprites.other['dream_world'].front_default"
            style="height: 150px;" alt="">
        </div>
        <div class="row" id="pkmn-sprite-container">
          <img class="pkmn-sprite" :src="pkmnData.sprites.back_default" alt="">
          <img class="pkmn-sprite" :src="pkmnData.sprites.front_default" alt="">
          <img class="pkmn-sprite" :src="pkmnData.sprites.front_shiny" alt="">
          <img class="pkmn-sprite" :src="pkmnData.sprites.back_shiny" alt="">
        </div>
      </div>
    </div>
    
    <div class="row">
      <h5>Evolutions</h5>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Detail',
  data () {
    return {
      url: 'https://pokeapi.co/api/v2/pokemon/',
      pkmnData: null,
      texts: [],
      evolutions: {
        evolutions1: [],
        evolutions2: []
      }
    }
  },
  mounted () {
    axios
      .get(this.url + this.$route.params.id)
      .then(response => {
        this.pkmnData = response.data
        console.log('infos sur le pkmn')
        console.log(this.pkmnData)

        var speciesUrl = this.pkmnData.species.url
        axios
          .get(speciesUrl)
          .then(response => {
            // !!! Voir ici pour les texts en fr !!!
            console.log("espece : ")
            console.log(response.data)
            response.data.flavor_text_entries.forEach((textData)=>{
              if(textData.language.name == "en")
                this.texts.push(textData)
            })
            
            var evolutionUrl = response.data.evolution_chain.url
            axios
              .get(evolutionUrl)
              .then(response => {
                // !!! Voir ici pour les évolutions !!!
                console.log("evolutions : ")
                console.log(response.data)
                // évolution niveau 1
                if(response.data.chain.evolves_to.length != 0)
                {
                  var evo1 = []
                  response.data.chain.evolves_to.forEach((evolution)=>{
                    var ev = {}
                    ev.name = evolution.species.name 
                    ev.trigger = evolution.evolution_details[0].trigger.name 
                    ev.level = evolution.evolution_details[0].min_level 
                    // console.log("ev :")
                    // console.log(ev)
                    evo1.push(ev)
                  })
                  this.evolutions.evolutions1 = evo1
                  // console.log("evo1 :")
                  // console.log(evo1)
                  // évolution niveau 2
                  if(response.data.chain.evolves_to[0].evolves_to.length != 0)
                  {
                    var evo2 = {}
                    evo2.name = response.data.chain.evolves_to[0].evolves_to[0].species.name 
                    evo2.trigger = response.data.chain.evolves_to[0].evolves_to[0].evolution_details[0].trigger.name 
                    evo2.level = response.data.chain.evolves_to[0].evolves_to[0].evolution_details[0].min_level 
                    // console.log("evo2 :")
                    // console.log(evo2)
                    this.evolutions.evolutions2 = evo2

                  }
                }
                
          })


          })
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

div #pkmn-image-container{
  margin-top: 150px;
}
</style>