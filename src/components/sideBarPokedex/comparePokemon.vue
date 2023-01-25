<template>
  <div class="TypeRelationsWindow">
    <button @click="setTypes"> Click..</button>
     
    <div class="TypeRelations">
      <div class="LeftType">   
        <div class="TypeNameLeft">
          <p> Your Attack Skillset: </p>
        </div>
        <div class="DoubleDmgLeft">
          <p>Sehr Effektiv: </p>
          <p v-for="(doubleDamageType, index) in doubleDamageTypes1" :key="index"> {{ doubleDamageType }} &nbsp; </p>
          <p v-for="(doubleDamageType, index) in doubleDamageTypes2" :key="index"> {{ doubleDamageType }} &nbsp; </p>
        </div>
        <div class="HalfDmgLeft">
          <p> Sehr Ineffektiv: </p>
          <p v-for="(weakType, index) in weakTypes" :key="index"> {{ weakType }} &nbsp; </p>
        </div>
        <div class="NoDmgLeft">
          <p> Harmlos: </p>
          <p v-for="(noDamageType, index) in noDamageTypes1" :key="index"> {{ noDamageType }} &nbsp; </p>
          <p v-for="(noDamageType, index) in noDamageTypes2" :key="index"> {{ noDamageType }} &nbsp; </p>
        </div>
      </div>

    </div> 
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
    
    doubleDamageTypes1: [],
    halfDamageTypes1: [],
    noDamageTypes1: [],

    doubleDamageTypes2: [],
    halfDamageTypes2: [],
    noDamageTypes2: [],

    weakTypes: [],

    DDTmsg: "",
    HDTmsg: "",
    NDTmsg: "",

    effectiveString: "",

   }
 },

methods: {
 
  setTypes(){

    for(let i = 0; i < this.übergebeneTypen.pokemonTypL.length; i++){
      this.myTypes[i] = this.übergebeneTypen.pokemonTypL[i].type.name;
      this.getTypeInfos(this.myTypes[i], i);
    }
    
    this.trimTypeRelations();
    this.displayTypeRelations();

    for(let i = 0; i < this.übergebeneTypen.pokemonTypR.length; i++){
      this.enemyTypes[i] = this.übergebeneTypen.pokemonTypR[i].type.name;
    }
  },

  async getTypeInfos(Type, number){
    
    await axios.get(`https://pokeapi.co/api/v2/type/${Type}`).then((response) => {
      let AttackenInformationen = response.data.damage_relations 
      let doubleDamageTo = AttackenInformationen.double_damage_to; 
      let halfDamageTo = AttackenInformationen.half_damage_to; 
      let noDamageTo = AttackenInformationen.no_damage_to;
      let Name = response.data.name;
      console.log(Name);
       
      if(doubleDamageTo != 0){
        for(let i = 0; i < doubleDamageTo.length; i++){
          if(number == 0){
            this.doubleDamageTypes1[i] = doubleDamageTo[i].name;
          }else{
            this.doubleDamageTypes2[i] = doubleDamageTo[i].name;
          }
        }
      }else{
        this.DDTmsg = "Keine Relation gefunden";
        console.log(this.DDTmsg);
      }   
      
      if(halfDamageTo != 0){
        for(let i = 0; i < halfDamageTo.length; i++){
        if(number == 0){
          this.halfDamageTypes1[i] = halfDamageTo[i].name;
        }else{
          this.halfDamageTypes2[i] = halfDamageTo[i].name;
        }       
      }
      }else{
        this.HDTmsg = "Keine Relation gefunden";
        console.log(this.HDTmsg);
      }
      
    
      if(noDamageTo != 0){
        for(let i = 0; i < noDamageTo.length; i++){
          if(number == 0){
            this.noDamageTypes1[i] = noDamageTo[i].name;
          }else{
            this.noDamageTypes2[i] = noDamageTo[i].name;
          }         
        } 
      }else{
        this.NDTmsg = "Keine Relation gefunden";
        console.log(this.NDTmsg);
    }
        
    })
  },

    displayTypeRelations(){
      console.log(this.doubleDamageTypes1);
      console.log(this.doubleDamageTypes2);
      console.log(this.weakTypes);
      console.log(this.noDamageTypes1);
      console.log(this.noDamageTypes2);
    },

    trimTypeRelations(){
      let k = 0;
      
      for(let i = 0; i < this.halfDamageTypes1; i++){
        for(let j = 0; j < this.halfDamageTypes2; j++){
          if(this.halfDamageTypes1[i] == this.halfDamageTypes2[j]){
            this.weakTypes[k] = this.halfDamageTypes1[i];
            k++;
          }
        }
      }

      if(this.weakTypes.length == 0){
        this.weakTypes[0] = "None";
      }

    },

 }
}
</script>

<style>
 
.TypeRelationsWindow{
  height: 200px;
  width: 100%;
}

.TypeRelations{
  display: flex;
  flex-direction: row;
  height: 200px;
  width: 100%;
}

  .LeftType{
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 200px;
  }

    .TypeNameLeft{
      width: 100%;
      height: 25%;
    }

    .DoubleDmgLeft{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

    .HalfDmgLeft{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

    .NoDmgLeft{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }


  .RightType{
    display: flex;
    flex-direction: column;
    width: 50%;
    height: 200px;
  }

    .TypeNameRight{
      width: 100%;
      height: 25%;
    }

    .DoubleDmgRight{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

    .HalfDmgRight{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

    .NoDmgRight{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

</style>