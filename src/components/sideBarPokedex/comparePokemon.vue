<template>

  <div class="test2">  
   <button @click="getData" type="button" :disabled="übergebenesBoolean == false">Dieses Pokemon Vergleichen</button> 
   <button @click="emitTest" type="button">EMIT TEST</button>

   <div v-if="this.compare === true">
     <img class="PokemonPicture" :src="pokeSpriteCompare.other.home.front_shiny" /> <br>
     Name:{{ pokeNameCompare  }} <br>
     Stats:
     <p v-for="(stats, zähler) in baseStatsCompare" :key="zähler">{{ stats.stat.name }} : {{ stats.base_stat }}</p>
     Kommt in diesen Spielen vor: <p v-for="(version, index) in pokeGenCompare" :key="index">{{ index + 1 }} : {{version.version.name}}</p>
     <p v-for="(typ, zähler) in pokemonTypCompare" :key="zähler">Typ {{zähler + 1}} : {{typ.type.name}}</p>
   </div>
  </div>

</template>


<script>
export default {
 props: ["übergebenesBoolean", "übergabePokeObjekt"],
 data(){ 
   return {
     pokeNameCompare: "",
     pokeGenCompare: [],
     pokeSpriteCompare: {},
     compare: false,
     baseStatsCompare: [],
     pokemonTypCompare: [],
   }
 },

 methods: {
   emitTest(){
     this.$emit("parentAufruf", 1000)
   },

   async getData(){ //API Call für Pokemon Daten
     this.pokeNameCompare = this.übergabePokeObjekt.name;
     this.pokeGenCompare = this.übergabePokeObjekt.game_indices;
     this.pokeSpriteCompare = this.übergabePokeObjekt.sprites;
     this.baseStatsCompare = this.übergabePokeObjekt.stats;
     this.pokemonTypCompare = this.übergabePokeObjekt.types;
     this.compare = true;
   },

 }
}

</script>

<style>

 .PokemonPicture{
   height: 200px;
   width: 200px;
 }

 .test2{
   background-color: #78ad75;
   margin-right: 5%;
   width: 45%;
   height: 80%;
   float: right;
   overflow: scroll;
 }

 .test2::-webkit-scrollbar{
   display: none;
 }
 
</style>