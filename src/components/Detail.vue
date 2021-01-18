<template>
  <div class="home" v-if="pkmnData !=null">

    <div class="row">
      <h2>{{ pkmnData.name | capitalize }}</h2>
      <p>#{{ pkmnData.id }}</p>
      <p>Height : {{ pkmnData.height*10 }} cm</p>
      <p>Weight : {{ pkmnData.weight*0.1 }} kg</p>
      <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sit ab perspiciatis consectetur deleniti quia! Rem ducimus aspernatur similique, provident quidem, accusantium ipsa, ullam vitae laboriosam modi quos veniam atque laborum.</p>
    </div>

    <div class="row">
      <img :src="pkmnData.sprites.front_default" alt="">
      <img :src="pkmnData.sprites.back_default" alt="">
      <img :src="pkmnData.sprites.other['official-artwork'].front_default"
        style="height: 150px;" alt="">
      <img :src="pkmnData.sprites.front_shiny" alt="">
      <img :src="pkmnData.sprites.back_shiny" alt="">
      <img :src="pkmnData.sprites.other['dream_world'].front_default"
        style="height: 150px;" alt="">
    </div>
  
    <div class="row">
      <p v-for="type in pkmnData.types"
        :key="type.slot">
        {{ type.type.name | capitalize }}
      </p>
    </div>
    
    <div class="row">
      <h3>Evolutions</h3>
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
</style>