<template>
  <div class="home">

    <div class="row">
      <h1>Detail</h1>
      <p>{{ $route.params.id }}</p>
      <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Sit ab perspiciatis consectetur deleniti quia! Rem ducimus aspernatur similique, provident quidem, accusantium ipsa, ullam vitae laboriosam modi quos veniam atque laborum.</p>
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
      pkmnData: [],
    }
  },
  mounted () {
    console.log('by mounted')
    console.log(this.url )
    console.log(this.$route.params.id)
    axios
      .get(this.url + this.$route.params.id)
      .then(response => {
        this.pkmnData = response.data.results
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