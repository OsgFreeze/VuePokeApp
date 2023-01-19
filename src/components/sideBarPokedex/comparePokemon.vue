<template>

   <div>  
    <button @click="getData" type="button" :disabled="übergebenesBoolean == false">Dieses Pokemon Vergleichen</button> 
    <button @click="emitTest" type="button">EMIT TEST</button>

    <div v-if="this.compare === true">
      <img class="PokemonPicture" :src="pokeSpriteCompare.other.home.front_shiny" /> <br>
      Name:{{ pokeNameCompare }} <br>
      Stats:
      <p v-for="(stats, zähler) in baseStatsCompare" :key="zähler">{{ stats.stat.name }} : {{ stats.base_stat }}</p>
      Kommt in diesen Spielen vor: <p v-for="(version, index) in pokeGenCompare" :key="index">{{ index + 1 }} : {{version.version.name}}</p>
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
      this.compare = true;
    },

  }
}

</script>

<style>

  .PokemonPicture{
    height: 250px;
    width: 250px;
  }

</style>