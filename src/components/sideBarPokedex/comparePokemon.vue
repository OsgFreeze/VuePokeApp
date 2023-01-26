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
    veryweakTypes: [],
    effectiveTypes: [],

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
      console.log(this.effectiveTypes);
      console.log(this.weakTypes);
      console.log(this.veryweakTypes);
    },

    trimTypeRelations(){
      
      // Wenn Beide Types half dmg gegen ein Typ haben
      let k = 0;
      
      for(let i = 0; i < this.halfDamageTypes1.length; i++){
        for(let j = 0; j < this.halfDamageTypes2.length; j++){
          if(this.halfDamageTypes1[i] == this.halfDamageTypes2[j]){
            this.veryweakTypes[k] = this.halfDamageTypes1[i];
            k++;
          }
        }
      }

      if(this.veryweakTypes.length == 0){
        this.veryweakTypes[0] = "None";
      }

      // Filter true double dmg Types für Typ1; wenn einer double einer half --> aussortieren

      let x = 0;

      for(let i = 0; i < this.doubleDamageTypes1.length; i++){
        for(let j = 0; j < this.halfDamageTypes2.length; j++){
          if(this.doubleDamageTypes1[i] != this.halfDamageTypes2[i]){
            this.effectiveTypes[x] = this.doubleDamageTypes1[i];   // Typ1 nur double Dmg & kein half Dmg --> effectiveTypes[]
            this.weakTypes[x] = this.halfDamageTypes1[i];    // Typ1 nur half Dmg & kein Double Dmg --> weakTypes[]
            x++;
          }
        }
      }

      // Filter true double dmg Types für Typ2 ; evtl diese Methode auslagern und 2 mal aufrufen mit Array als übergabewert

      let y = 1;

      for(let i = 0; i < this.doubleDamageTypes2.length; i++){
        for(let j = 0; j < this.halfDamageTypes1.length; j++){
          if(this.doubleDamageTypes2[i] != this.halfDamageTypes1[i]){
            this.effectiveTypes[x+y] = this.doubleDamageTypes2[i];  // Typ2 nur double Dmg & kein half Dmg --> effectiveTypes[]
            this.weakTypes[x+y] = this.halfDamageTypes2[i];   // Typ2 nur half Dmg & kein Double Dmg --> weakTypes[]
            y++;
          }
        }
      }

      // Filter: wenn irgendwo No Dmg Type existiert --> Relation aus allen anderen Arrays Löschen

      // Schleife nur is 1 da ein Typ maximal ein no Dmg Type haben kann
      for(let i = 0; i <= 1; i++){
        for(let j = 0; j < this.effectiveTypes.length; j++){
          if(this.noDamageTypes1[i] == this.effectiveTypes[j] || this.noDamageTypes2[i] == this.effectiveTypes[j]){
            this.effectiveTypes[j] = null;
          }
        }
        for(let k = 0; k <= 1; k++){
          if(this.noDamageTypes1[i] == this.weakTypes[k] || this.noDamageTypes2 == this.weakTypes[k]){
            this.weakTypes[k] = null;
          }
        }
        for(let n = 0; n <= 1; n++){
          if(this.noDamageTypes1[i] == this.veryweakTypes[n] || this.noDamageTypes2 == this.veryweakTypes[n]){
            this.veryweakTypes[n] = null;
          }
        }
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