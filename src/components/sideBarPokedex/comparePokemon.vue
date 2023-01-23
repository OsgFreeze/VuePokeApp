<template>
  <div class="DisplayRightSide">  
   <button @click="getData" type="button" :disabled="pokeDatenAnzeigenCompare == false">Pokemon Vergleichen</button> <!-- Button um getData Methode aufzurufen-->

   <div v-if="this.pokeDatenAnzeigen === true"> <!-- if Abfrage weil Api Call noch nicht gemacht worden ist -->
     <img class="PokemonPicture" :src="pokeSpriteCompare.other.home.front_shiny" /> <br> <!-- Bild wird angezeigt -->
     Name:{{ pokeNameCompare  }} <br>
     Stats:
     <p v-for="(stats, zähler) in baseStatsCompare" :key="zähler">{{ stats.stat.name }} : {{ stats.base_stat }}</p>
     Kommt in diesen Spielen vor: <p v-for="(version, index) in pokeGenCompare" :key="index">{{ index + 1 }} : {{version.version.name}}</p> <!-- Mit For Schleife läuft man durch das Objekt oder durch die Liste um alle Daten anzuzeigen -->
     <p v-for="(typ, zähler) in pokemonTypCompare" :key="zähler">Typ {{zähler + 1}} : {{typ.type.name}}</p>
   </div>
  </div>
</template>

<script>
export default {
 props: ["pokeDatenAnzeigenCompare", "übergabePokeObjekt"],
 data(){ 
   return {
     pokeNameCompare: "",
     pokeGenCompare: [],
     pokeSpriteCompare: {},
     pokeDatenAnzeigen: false,
     baseStatsCompare: [],
     pokemonTypCompare: [],
   }
 },

 methods: {
 
   async getData(){ //API Call für Pokemon Daten
     this.pokeNameCompare = this.übergabePokeObjekt.name;
     this.pokeGenCompare = this.übergabePokeObjekt.game_indices;
     this.pokeSpriteCompare = this.übergabePokeObjekt.sprites;
     this.baseStatsCompare = this.übergabePokeObjekt.stats;
     this.pokemonTypCompare = this.übergabePokeObjekt.types;
     this.pokeDatenAnzeigen = true;
   },

 }
}
</script>

<style>
 .PokemonPicture{
   height: 200px;
   width: 200px;
 }

 .DisplayRightSide{
   background-color: #78ad75;
   margin-right: 5%;
   width: 45%;
   height: 80%;
   float: right;
   overflow: scroll;
 }

 .DisplayRightSide::-webkit-scrollbar{
   display: none;
 }
</style>