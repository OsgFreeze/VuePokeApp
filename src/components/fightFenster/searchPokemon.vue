<template>  
    <div> 
        <input id="eingabeId" type="number" v-model="PokeNumber">     
    </div>

    <div class="buttons">
        <button @click="setNrPlus" > Hoch ↑ </button>
        <button @click="setNrMinus"> Runter ↓ </button> 
        &nbsp;
        <button @click="loadApiFromNumber"> Eingabe </button>
      </div>
    
  </template>
  
  <script>

    import myComp from './components/myComp.vue';

      export default {
      name: 'searchPokemon',   
      //props: ['übergebeneVariable'],
      data() {
        return {
            pokeNumber: 1,
            pokeName: "",
        }
      }, 

      methods: {
        async loadApiNumber(){
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
                console.log(response);
                const data = response.data;   
                this.pokeName = data.name; 
      })
    },
    
    async setNrPlus(){
      this.PokeNumber = (this.PokeNumber + 1);
      this.loadApiFromNumber();
 
    },
    async setNrMinus(){
      this.PokeNumber = (this.PokeNumber - 1);
      this.loadApiFromNumber();
    },
  }
}
  </script>
  
  <style>
  </style>