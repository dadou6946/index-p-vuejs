<template>
  <div>
    <transition name="fade">
      <div id="loader-container" v-if=showLoader>
        <img class="img-loader" id="loader" :src="'/loader.png'">
      </div> 
    </transition>

    <transition name="fade">
      <div class="detail" v-if="!showLoader">

        <!-- CONTENANT INFO ET IMAGES -->
        <div class="row">

          <!-- COLONNE DE GAUCHE ------------------------------------------------------------>
          <div class="col m6">

            <!-- BOUTONS ET IDENTIFIANTS -->
            <div class="row">
              <div class="col m4 change-button-content">
                <button class="waves-effect waves-light btn-floating red"
                  :disabled="pkmnData.id==1"
                  @click="redirection(pkmnData.id -1)">
                  <i class="material-icons">keyboard_arrow_left</i>
                </button>
              </div>
              <div class="col m4">
                <h4 id="pkmn-id">#{{ pkmnData.id }}</h4>
              </div>
              <div class="col m4 change-button-content">
                <button class="waves-effect waves-light btn-floating red"
                  :disabled="pkmnData.id==898"
                  @click="redirection(pkmnData.id + 1)">
                  <i class="material-icons">keyboard_arrow_right</i>
                </button>
              </div>
            </div>

            <!-- NOM ET DIMENSIONS -->
            <div class="row">
              <h3 id="pkmn-name">{{ pkmnData.name | capitalize }}</h3>
              <div class="col m12 dimensions-content">
                <p id="pkmn-height">Height : {{ pkmnData.height*10 }} cm</p>
                <p id="pkmn-weight">Weight : {{ pkmnData.weight*0.1 }} kg</p>
              </div>
            </div>

            <!-- TYPES -->
            <div class="row" id="pkmn-types">
              <h5>Type<span v-if="pkmnData.types.length==2">s</span> :</h5>
              <div v-for="type in pkmnData.types"
                :key="type.slot"
                class="type-icon" 
                :class="'type-'+type.type.name">
                {{ type.type.name | capitalize }}
              </div>
            </div>        
            
          </div>

          <!-- COLONNE DE DROITE ------------------------------------------------------------>
          <div class="col m6">
            
            <!-- PHOTOS -->
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
        <!-- FIN CONTENANT INFO ET IMAGES -->
        
        <!-- CONTENANT CAPACITES -->
        <div class="row capacities-content">
          <!-- BOUTONS AVEC NOMS -->
          <div class="col m6">
            <!-- CAPACITES -->
            <div class="row" id="pkmn-abilities">
              <div class="col m12">
                <h5>
                  <span v-if="pkmnData.abilities.length==1">Ability</span>
                  <span v-if="pkmnData.abilities.length!=1">Abilities</span> :
                </h5>
                <p class="ability-explanations-text">Click on an ability for more details.</p>
                <div class="ability-button-content">
                  <button class="btn-small waves-effect waves-light"
                    :class="'type-'+pkmnData.types[0].type.name"
                    v-for="(ability, index) in abilities" 
                    :key="index"
                    @click="showAbility(ability.name, ability.effect)">
                    {{ ability.name | capitalize }}
                  </button>              
                </div>
              </div>
            </div>
          </div>

          <!-- AFFICHAGE DU DETAIL DES CAPACITES -->
          <div class="col m6">
            <div class="row">
              <transition name="fade">
              <div class="col m12 ability-text-content" v-if=showAbilityText>
                <div class="card grey lighten-3">
                  <div class="card-content">
                    <span class="card-title">{{ abilityTitle | capitalize }}</span>
                    <p>{{ abilityText }}</p>
                  </div>
                </div>
              </div>
              </transition>
            </div>
          </div>
        </div>
        <!-- FIN CONTENANT CAPACITES -->

        <!-- CONTENANT TEXTES PKDEX -->
        <div class="row">
          <!-- COLONNE DE GAUCHE -->
          <div class="col m6">
            <!-- TEXTES DE PKMN -->
            <div class="row" id="pkmn-texts">
              <div class="col m4 offset-m4">
                <h5>Pokedex texts</h5>
                <p  v-show="showTextsButton">Click on a version to show details.</p>
              </div>
              <div class="col m4">
                <a class="btn-floating btn waves-effect waves-light red text-appear-button left" 
                    @click="manageText()">
                  <i class="material-icons" v-if="!showTextsButton">add</i>
                  <i class="material-icons" v-if="showTextsButton">remove</i>
                </a>
              </div>
              <div class="pkmn-text-container col m10 offset-m1" v-show="showTextsButton" style="margin-top: 40px;">
                <button class="pkmn-text-button btn-small waves-effect waves-light "
                  @click="changeCurrentText(text)" 
                  v-for="(text,index) in texts"
                  style="margin: 2px;" 
                  :key="index"
                  :class="'button-' + text.version.name">
                  {{ text.version.name }}
                </button>
              </div>
            </div>
          </div>

          <!-- COLONNE DE DROITE -->
          <div class="col m6" style="margin-top: 150px">
            <!-- Texte pokedex affiché -->
            <div class="row">
              <transition name="fade">
                <div class="col m12 pokedex-text-content"
                  v-if="currentText !=''">
                  <div class="card grey lighten-3">
                    <div class="card-content">
                      <span class="card-title">{{ currentVersion | capitalize }}</span>
                      <p>{{ currentText }}</p>
                    </div>
                  </div>
                </div>
              </transition>
            </div>
          </div>

        </div>
        <!-- FIN CONTENANT TEXTES PKDEX -->
        
        <!-- <div class="row"> -->
          <!-- ATTAQUES -->
          <!-- <div id="pkmn-attacks">
            <h5>Attacks :</h5>
          </div>
        </div> -->
        
        <!------------ CONTENANT EVOLUTIONS ------------>
        <div class="row">

          <h5>Evolutions</h5>
          <div class="row">

            <!------------ Base ------------>
            <div class="col m4 pkmn-base" v-if="evolutions.base.image != ''">
              <div class="pkmn-card card lighten-5" :class="'type-'+pkmnData.types[0].type.name">
                <div class="card-content white-text">
                  <img v-bind:src="evolutions.base.image"
                    style="height: 70px;" 
                    alt="">
                  <p>Basic pokemon</p>
                </div>
                <div class="card-action">
                  <button class="btn red" @click="redirection(evolutions.base.name)">{{ evolutions.base.name | capitalize }}</button>
                </div>
              </div>
            </div>

            <!------------ EVOLUTIONS 1 ------------>
            <div class="col m4 pkmn-evolution-1 pkmn-card" 
              v-for="(evo1, index) in evolutions.evolutions1" 
              :key="index">
              <div class="pkmn-card card" :class="'type-'+pkmnData.types[0].type.name">
                <div class="card-content white-text">
                  <img v-if="evo1.image != ''" :src="evo1.image"
                    style="height: 70px;" 
                    alt="">
                  <p>Evolution 1</p>
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
                  <button class="btn red" @click="redirection(evo1.name)">{{ evo1.name | capitalize }}</button>
                </div>
              </div>
            </div>

            <!------------ EVOLUTIONS 2 ------------>
            <div class="col m4 pkmn-evolution-2 pkmn-card" v-for="(evo2, index) in evolutions.evolutions2" :key="index">
              <div class="pkmn-card card" :class="'type-'+pkmnData.types[0].type.name">
                <div class="card-content white-text">
                  <img v-if="evo2.image != ''" :src="evo2.image"
                    style="height: 70px;" 
                    alt="">
                  <p>Evolution 2</p>
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
                  <button class="btn red" @click="redirection(evo2.name)">{{ evo2.name | capitalize }}</button>
                </div>
              </div>
            </div>

          </div>

        </div>
        <!------------ FIN CONTENANT EVOLUTIONS ------------>
      </div>
    </transition>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Detail',
  data () {
    return {
      showLoader: true,
      pkmnUrl: 'https://pokeapi.co/api/v2/pokemon/',
      pkmnData: null,
      abilities: [],
      showAbilityText: false,
      abilityTitle: '',
      abilityText: '',
      texts: [],
      showTextsButton: false,
      currentText: '',
      currentVersion: '',
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
      // Reset et chargement des données sur le nouveau
      this.resetData()
      this.recupererDonnes()
    },
    // Affiche le texte correspondant à la capacité dans la zone de texte dédié au clique sur le bouton de la capacité
    showAbility: function(ability, effect){
      if(this.showAbilityText == false)
      {
        this.abilityTitle = ability
        this.abilityText = effect
        this.showAbilityText = true
      }
      else
      {
        if(this.abilityTitle == ability)
        {
          this.showAbilityText = false
          this.abilityTitle = ''
          this.abilityText = ''
        }
        else
        {
          this.abilityTitle = ability
          this.abilityText = effect
        }
      }
    },
    // Gère l'affiche du texte de pkdex en appuyant sur le bouton + / -
    manageText: function(){
      this.showTextsButton = !this.showTextsButton
      if(this.showTextsButton == false)
        this.currentText = ''
    },
    // Met à jour le texte de pokedex courant et le nom de la version de provenance
    changeCurrentText: function(text){
      this.currentText = text.flavor_text
      this.currentVersion = text.version.name
    },
    resetData: function(){
      this.showLoader = true,
      this.pkmnData = null,
      this.abilities = [],
      this.showAbilityText = false,
      this.abilityTitle = '',
      this.abilityText = '',
      this.texts = [],
      this.showTextsButton = false,
      this.currentText = '',
      this.currentVersion = '',
      this.evolutions = {
        base: {
          name: '',
          image: ''
        },
        evolutions1: [],
        evolutions2: []
      }
    },
    recupererDonnes: function(){
      // Remise à vide des textes
      this.texts = []
      // Appel api pour les données du pkmn en cours
      axios
      .get(this.pkmnUrl + this.$route.params.pkmn)
      .then(response => {
        this.pkmnData = response.data
        var speciesUrl = this.pkmnData.species.url

        // Récupération des données pour les habilités
        // Pour chacune des habilités
        response.data.abilities.forEach((ability)=>{
          var newAbility = {}
          // appel api avec ability.url pour récupérer les données pour chaque ability
          axios
          .get(ability.ability.url)
          .then(response => {
            
            newAbility.name = response.data.name
            newAbility.pokemon = response.data.pokemon

            response.data.effect_entries.forEach((abilityData)=>{
              // On récupère uniquement les données en anglais
              if(abilityData.language.name =="en"){
                newAbility.effect = abilityData.effect
                newAbility.short_effect = abilityData.short_effect
              }
            })

          })
          this.abilities.push(newAbility)
        })

        // Appel api pour les données d'especes
        axios
          .get(speciesUrl)
          .then(response => {
            // Récupération des textes pkdex
            response.data.flavor_text_entries.forEach((textData)=>{
              // On récupère les textes en anglais et les versions qui ne sont pas lets-go, shield et sword...
              if(textData.language.name == "en" 
                && !textData.version.name.includes("lets-go-") 
                && !textData.version.name.includes("sword") 
                && !textData.version.name.includes("shield")
              )
              {
                textData.flavor_text = textData.flavor_text.replace('\f', ' ')
                this.texts.push(textData)
              }  
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
      setTimeout(function () { this.showLoader = false;  }.bind(this), 500)
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
  margin-top: 70px;
}

div.dimensions-content {
  margin-top: 15px;
}

div.change-button-content{
  padding-top: 20px;
}
a.text-appear-button{
  margin-top: 15px;
}

/*boutons textes*/
button.pkmn-text-button {
  text-shadow: 1px 1px 2px rgb(0 0 0 / 70%);
}

button.button-red { background-color: #f44336 ; }
button.button-blue, button.button-soulriver { background-color: #2196f3; }
button.button-yellow { background-color: #ffeb3b ; }
button.button-gold, button.button-heartgold { background-color: #ffca28 ; }
button.button-silver { background-color: #90a4ae ; }
button.button-crystal { background-color: #b2ebf2 ; }
button.button-ruby, button.button-omega-ruby { background-color: #b71c1c ; }
button.button-sapphire, button.button-omega-sapphire { background-color: #3f51b5 ; }
button.button-emerald { background-color: #00e676 ; }
button.button-firered { background-color: #e53935 ; }
button.button-leafgreen { background-color: #4caf50 ; }
button.button-diamond { background-color: #9e9e9e ; }
button.button-pearl { background-color: #eeeeee; }
button.button-platinum { background-color: #757575 ; }
button.button-black, button.button-black-2 { background-color: #000000 ; }
button.button-white, button.button-white-2 { background-color: #ffffff ; }
button.button-x { background-color: #1a237e ; }
button.button-y { background-color: #283593 ; }
button.button-sun, button.button-ultra-sun { background-color: #ffff00 ; }
button.button-moon, button.button-ultra-moon { background-color: #bdbdbd ; }

/*Capacités*/
div.ability-button-content {
  margin-top: 20px;
}

div.capacities-content {
  height: 215px
}
div.ability-text-content {
  margin-top: 50px;
}


/*Loader*/
#loader {
  margin-top: 100px;
}
div#loader-container{
  height: 800px;
  width: 100%;
}

/*Animations*/
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

/*couleurs Evolutions */
div.pkmn-card  {
  height: 260px;
}

div.pkmn-base {
  filter: brightness(140%);
}

div.pkmn-evolution-1 {
  filter: brightness(120%);
}

/*Types*/
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