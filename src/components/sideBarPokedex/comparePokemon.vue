<template>
  <div class="TypeRelations">
    <button @click="setTypes"> Click..</button>
    <p> {{ this.myTypes }} is effective against {{  }} </p>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  
 props: ["übergebeneTypen"],
 data(){ 
   return {

    myTypes: [],
    enemyTypes: [], 
    allTypes: [],
    
    doubleDamageTypes: [],
    halfDamageTypes: [],
    noDamageTypes: [],

   }
 },

 methods: {
 
    setTypes(){

      for(let i = 0; i < this.übergebeneTypen.pokemonTypL.length; i++){
        this.myTypes[i] = this.übergebeneTypen.pokemonTypL[i].type.name;
        this.allTypes[i] = this.übergebeneTypen.pokemonTypL[i].type.name;
      }
      
      for(let i = 0; i < this.übergebeneTypen.pokemonTypR.length; i++){
        this.enemyTypes[i] = this.übergebeneTypen.pokemonTypR[i].type.name;
        this.allTypes[i+this.übergebeneTypen.pokemonTypL.length] = this.übergebeneTypen.pokemonTypR[i].type.name; 
      }
      console.log(this.allTypes);
      this.getTypeInfos();
    },

    async getTypeInfos(){
      
      for(let i = 0; i < this.allTypes.length; i++){
        await axios.get(`https://pokeapi.co/api/v2/type/${this.allTypes[i]}`).then((response) => {
          let AttackenInformationen = response.data.damage_relations 
          let doubleDamageTo = AttackenInformationen.double_damage_to; 
          let halfDamageTo = AttackenInformationen.half_damage_to; 
          let noDamageTo = AttackenInformationen.no_damage_to;
          let Name = response.data.name;
          console.log(Name);

          for(let i = 0; i < doubleDamageTo.length; i++){
            this.doubleDamageTypes[i] = doubleDamageTo[i].name;
          }
          console.log(this.doubleDamageTypes);

          for(let i = 0; i < halfDamageTo.length; i++){
            this.halfDamageTypes[i] = halfDamageTo[i].name;
          }
          console.log(this.halfDamageTypes);

          for(let i = 0; i < noDamageTo.length; i++){
            this.noDamageTypes[i] = noDamageTo[i].name;
          }
          console.log(this.noDamageTypes);
        })
      }
    },

    displayTypeRelations(){

    },

 }
}
</script>

<style>
 

</style>