<template>
  <div class="container">
    <h1 class="text-center">Pokemon</h1>
    <hr>
    <div class="row" v-for="(pokemons, index) in groupedPokemon" :key="'pokemon'+index">
    
    <div v-for="(pokemon,index) in pokemons" :key="'poke'+index" class="col-lg-4">
    <div class="card mb-5 bg">
      <div class="card-body pb-5">
        <img :src="imageUrl + pokemon.id + '.png'" width="96" height="96">
                <h1>{{pokemon.name}}</h1>
                <p>{{pokemon.url}}</p>
      </div>
    </div>
    </div>
    </div>
    <div class="row">
      <nav aria-label="...">
        <ul class="pagination">
          <li class="page-item" v-if="previousUrl !== null">
            <a class="page-link" href="javascript:void(0)" tabindex="-1" @click="previous">Previous</a>
          </li>
          <li class="page-item">
            <a class="page-link" href="javascript:void(0)" @click="next">Next</a>
          </li>
        </ul>
</nav>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash'
export default {
  data(){
    return {
      imageUrl:'https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/',
      apiUrl : 'https://pokeapi.co/api/v2/pokemon/',
      pokemons:[],
      nextUrl: '',
      currentUrl: '',
      previousUrl:'',
    }
  },
  methods:{
   async fetchData(){
   return await axios.get(this.currentUrl)
      .then((res) => {
        if(res.status === 200)
          this.nextUrl = res.data.next;
          this.previousUrl = res.data.previous;
        
          res.data.results.forEach(pokemon => {
            pokemon.id = pokemon.url.split('/')
              .filter(function(part){
                return !!part
              }).pop();
          this.pokemons.push(pokemon);
        })
            
      })
      .catch((err)=>{
        console.log(err);
      });
     
    },
    next(){
     
      this.currentUrl =  this.nextUrl;
      this.pokemons = [];
      this.fetchData();
    },
    previous(){
       this.currentUrl =  this.previousUrl;
      this.pokemons = [];
      this.fetchData();
    }
  },
  created(){                              
    this.currentUrl = this.apiUrl;
    this.fetchData();
  },
  computed:{
  groupedPokemon() {
    return _.chunk(this.pokemons, 3)  
  }
},

}

</script>

