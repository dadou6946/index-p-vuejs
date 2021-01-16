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
    }
  },
  mounted () {
    axios
      .get(this.url + this.$route.params.id)
      .then(response => {
        this.pkmnData = response.data
        console.log(this.pkmnData)
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