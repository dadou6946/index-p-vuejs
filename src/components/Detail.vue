<template>
  <div class="home" v-if="pkmnData !=null">

    <div class="row">

      <div class="col m6">

        <div class="row">
          <div class="col m4 change-button-content">
            <button class="btn-floating"
              :disabled="pkmnData.id==1"
              @click="redirection(pkmnData.id -1)">
              <i class="material-icons">keyboard_arrow_left</i>
            </button>
          </div>
          <div class="col m4">
            <h4 id="pkmn-id">#{{ pkmnData.id }}</h4>
          </div>
          <div class="col m4 change-button-content">
            <button class="btn-floating"
              :disabled="pkmnData.id==898"
              @click="redirection(pkmnData.id + 1)">
              <i class="material-icons">keyboard_arrow_right</i>
            </button>
          </div>
        </div>

        <h3 id="pkmn-name">{{ pkmnData.name | capitalize }}</h3>
        <p id="pkmn-height">Height : {{ pkmnData.height*10 }} cm</p>
        <p id="pkmn-weight">Weight : {{ pkmnData.weight*0.1 }} kg</p>

        <div id="pkmn-types">
          <h5>Type<span v-if="pkmnData.types.length==2">s</span> :</h5>
          <div v-for="type in pkmnData.types"
            :key="type.slot"
            class="type-icon" 
            :class="'type-'+type.type.name">
            {{ type.type.name | capitalize }}
          </div>
        </div>

        <div id="pkmn-abilities">
          <h5>
            <span v-if="pkmnData.abilities.length==1">Ability</span>
            <span v-if="pkmnData.abilities.length==2">Abilities</span> :
          </h5>
          <div v-for="(ability, index) in pkmnData.abilities" 
            :key="index">
            {{ ability.ability.name | capitalize }}
          </div>
        </div>
        <div id="pkmn-texts">
          <h5>Pokedex texts :</h5>
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
    
    <!------------ EVOLUTIONS ------------>
    <div class="row">

      <h5>Evolutions</h5>
      
      <!------------ Base ------------>
      <div class="row" v-if="evolutions.base.image != ''">
        <div class="col m4 pkmn-base">
          <div class="card teal lighten-5">
            <div class="card-content white-text">
              <img v-bind:src="evolutions.base.image"
                style="height: 70px;" 
                alt="">
            </div>
            <div class="card-action">
              <button class="btn" @click="redirection(evolutions.base.name)">{{ evolutions.base.name | capitalize }}</button>
            </div>
          </div>
        </div>
      </div>

      <!------------ EVOLUTIONS 1 ------------>
      <div class="row" v-if="evolutions.evolutions1.length !=0">
        <div class="col m4 pkmn-evolution-1" v-for="(evo1, index) in evolutions.evolutions1" :key="index">
          <div class="card teal lighten-4">
            <div class="card-content white-text">
              <img v-if="evo1.image != ''" :src="evo1.image"
                style="height: 70px;" 
                alt="">
              <p>Trigger : {{ evo1.trigger }}</p>
              <p v-if="evo1.min_level != null">Level : {{ evo1.min_level }}</p>
              <p v-if="evo1.item != null">Item : {{ evo1.item }}</p>
              <p v-if="evo1.min_happiness != null">Min happiness : {{ evo1.min_happiness }}</p>
              <p v-if="evo1.time_of_day != null">Time of the day : {{ evo1.time_of_day }}</p>
              <p v-if="evo1.min_affection != null">Min affection : {{ evo1.min_affection }}</p>
              <p v-if="evo1.known_move_type != null">Known move Type : {{ evo1.known_move_type }}</p>
              <p v-if="evo1.held_item != null">Held item : {{ evo1.held_item }}</p>
            </div>
            <div class="card-action">
              <button class="btn" @click="redirection(evo1.name)">{{ evo1.name | capitalize }}</button>
            </div>
          </div>
        </div>
      </div>  

      <!------------ EVOLUTIONS 2 ------------>
      <div class="row" v-if="evolutions.evolutions2.length !=0">
        <div class="col m4 pkmn-evolution-2" v-for="(evo2, index) in evolutions.evolutions2" :key="index">
          <div class="card teal lighten-3">
            <div class="card-content white-text">
              <img v-if="evo2.image != ''" :src="evo2.image"
                style="height: 70px;" 
                alt="">
              <p>Trigger : {{ evo2.trigger }}</p>
              <p v-if="evo2.min_level != null">Level : {{ evo2.min_level }}</p>
              <p v-if="evo2.item != null">Item : {{ evo2.item }}</p>
              <p v-if="evo2.min_happiness != null">Min happiness : {{ evo2.min_happiness }}</p>
              <p v-if="evo2.time_of_day != null">Time of the day : {{ evo2.time_of_day }}</p>
              <p v-if="evo2.min_affection != null">Min affection : {{ evo2.min_affection }}</p>
              <p v-if="evo2.known_move_type != null">Known move Type : {{ evo2.known_move_type }}</p>
              <p v-if="evo2.held_item != null">Held item : {{ evo2.held_item }}</p>
            </div>
            <div class="card-action">
              <button class="btn" @click="redirection(evo2.name)">{{ evo2.name | capitalize }}</button>
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Detail',
  data () {
    return {
      pkmnUrl: 'https://pokeapi.co/api/v2/pokemon/',
      pkmnData: null,
      texts: [],
      evolutions: {
        base: {
          name: '',
          image: ''
        },
        evolutions1: [],
        evolutions2: []
      }
    }
  },
  methods: {
    // Fonction pour rediriger vers une autre page
    redirection: function(pkmn){
      // redirection
      this.$router.push({ name: 'Detail', params: { pkmn: pkmn } })
      // chargement des données sur le nouveau
      this.recupererDonnes()
    },
    recupererDonnes: function(){
      // Appel api pour les données du pkmn en cours
      axios
      .get(this.pkmnUrl + this.$route.params.pkmn)
      .then(response => {
        this.pkmnData = response.data
        var speciesUrl = this.pkmnData.species.url

        // Appel api pour les données d'especes
        axios
          .get(speciesUrl)
          .then(response => {
            // !!! Voir ici pour les texts en fr !!!
            console.log("espece : ")
            console.log(response.data)
            // Récupération des textes
            response.data.flavor_text_entries.forEach((textData)=>{
              if(textData.language.name == "en")
                this.texts.push(textData)
            })

            // Appel api pour les données de chaine d'écolution
            var evolutionUrl = response.data.evolution_chain.url
            axios
              .get(evolutionUrl)
              .then(response => {
                // évolution niveau 1
                this.evolutions.base.name = response.data.chain.species.name
                // Recherche de la l'image du pkmn de base
                axios
                  .get(this.pkmnUrl + response.data.chain.species.name)
                  .then(response =>{
                    this.evolutions.base.image = response.data.sprites.other['dream_world'].front_default
                  })
                // Récupération des données d'évolution 1
                if(response.data.chain.evolves_to.length != 0)
                {
                  response.data.chain.evolves_to.forEach((evolution, index)=>{
                    // Objet de base vide
                    this.$set(this.evolutions.evolutions1, index, {name: '', image:''})
                    // Nom du pkmn
                    this.$set(this.evolutions.evolutions1[index], 'name', evolution.species.name)
                    // Recherche de la l'image du pkmn évolution 1
                    axios
                      .get(this.pkmnUrl + evolution.species.name)
                      .then(response =>{
                        // s'il y a un artwork amélioré on récupère son url
                        if(response.data.sprites.other['dream_world'].front_default != null)
                          this.$set(this.evolutions.evolutions1[index], 'image', response.data.sprites.other['dream_world'].front_default)
                        // sinon on prend celle de l'image officielle
                        else
                          this.$set(this.evolutions.evolutions1[index], 'image', response.data.sprites.other['official-artwork'].front_default)
                      })
                    // Trigger
                    this.$set(this.evolutions.evolutions1[index], 'trigger', evolution.evolution_details[0].trigger.name)
                    // Niveau minimum
                    if(evolution.evolution_details[0].min_level != null)
                      this.$set(this.evolutions.evolutions1[index], 'min_level', evolution.evolution_details[0].min_level)
                    // Objet
                    if(evolution.evolution_details[0].item != null)
                      this.$set(this.evolutions.evolutions1[index], 'item', evolution.evolution_details[0].item.name)
                    // joie minimum
                    if(evolution.evolution_details[0].min_happiness != null)
                      this.$set(this.evolutions.evolutions1[index], 'min_happiness', evolution.evolution_details[0].min_happiness)
                    // jour/nuit
                    if(evolution.evolution_details[0].time_of_day != '')
                      this.$set(this.evolutions.evolutions1[index], 'time_of_day', evolution.evolution_details[0].time_of_day)
                    // Affection minimum
                    if(evolution.evolution_details[0].min_affection != null)
                      this.$set(this.evolutions.evolutions1[index], 'min_affection', evolution.evolution_details[0].min_affection)
                    // known move type 
                    if(evolution.evolution_details[0].known_move_type != null)
                      this.$set(this.evolutions.evolutions1[index], 'known_move_type', evolution.evolution_details[0].known_move_type.name)
                    // Objet porté
                    if(evolution.evolution_details[0].held_item != null)
                      this.$set(this.evolutions.evolutions1[index], 'held_item', evolution.evolution_details[0].held_item.name)
                  })
                  // évolution niveau 2
                  if(response.data.chain.evolves_to[0].evolves_to.length != 0)
                  {
                    response.data.chain.evolves_to[0].evolves_to.forEach((evolution2, index)=>{
                      // Objet de base vide
                      this.$set(this.evolutions.evolutions2, index, {name: '', image:''})
                      // Nom du pkmn
                      this.$set(this.evolutions.evolutions2[index], 'name', evolution2.species.name)
                      // Recherche de la l'image du pkmn évolution 1
                      axios
                        .get(this.pkmnUrl + evolution2.species.name)
                        .then(response =>{
                          // s'il y a un artwork amélioré on récupère son url
                          if(response.data.sprites.other['dream_world'].front_default != null)
                            this.$set(this.evolutions.evolutions2[index], 'image', response.data.sprites.other['dream_world'].front_default)
                          // sinon on prend celle de l'image officielle
                          else
                            this.$set(this.evolutions.evolutions2[index], 'image', response.data.sprites.other['official-artwork'].front_default)
                        })
                      // Trigger
                      this.$set(this.evolutions.evolutions2[index], 'trigger', evolution2.evolution_details[0].trigger.name)
                      // Niveau minimum
                      if(evolution2.evolution_details[0].min_level != null)
                        this.$set(this.evolutions.evolutions2[index], 'min_level', evolution2.evolution_details[0].min_level)
                      // Objet
                      if(evolution2.evolution_details[0].item != null)
                        this.$set(this.evolutions.evolutions2[index], 'item', evolution2.evolution_details[0].item.name)
                      // joie minimum
                      if(evolution2.evolution_details[0].min_happiness != null)
                        this.$set(this.evolutions.evolutions2[index], 'min_happiness', evolution2.evolution_details[0].min_happiness)
                      // jour/nuit
                      if(evolution2.evolution_details[0].time_of_day != '')
                        this.$set(this.evolutions.evolutions2[index], 'time_of_day', evolution2.evolution_details[0].time_of_day)
                      // Affection minimum
                      if(evolution2.evolution_details[0].min_affection != null)
                        this.$set(this.evolutions.evolutions2[index], 'min_affection', evolution2.evolution_details[0].min_affection)
                      // known move type 
                      if(evolution2.evolution_details[0].known_move_type != null)
                        this.$set(this.evolutions.evolutions2[index], 'known_move_type', evolution2.evolution_details[0].known_move_type.name)
                      // Objet porté
                      if(evolution2.evolution_details[0].held_item != null)
                        this.$set(this.evolutions.evolutions2[index], 'held_item', evolution2.evolution_details[0].held_item.name)
                    })
                  }
                }
          })
        })
      })
    }
  },
  // created () {
  mounted () {
    this.recupererDonnes()
    
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

div.change-button-content{
  padding-top: 20px;
}
</style>