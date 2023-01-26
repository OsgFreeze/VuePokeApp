<template>
  <div class="TypeRelationsWindow">
    <div class="ViewSkillset">
      <button class="ViewSkillsetButton" @click="setTypes"> View Skillset</button>
    </div>
    <p style="font-size: 20px; text-decoration: underline"> Your Attack Skillset: </p>
     
    <div class="TypeRelations">
      <div class="LeftType">   
        <div class="SehrEffektiv">
          <div class="SETitle">
            <p style="font-size: large">Sehr Effektiv: </p>
          </div>
          <div class="color1"></div>
          <div class="SETypes">
            <p v-for="(veryeffectiveType, index) in veryeffectiveTypes" :key="index"> {{ veryeffectiveType }} </p>
          </div>   
        </div>
        <div class="Effektiv">
          <div class="ETitle">
            <p style="font-size: large"> Effektiv: </p>
          </div>
          <div class="color2"></div> 
          <div class="ETypes">
            <p v-for="(finalEffectiveType, index) in finalEffectiveTypes" :key="index"> {{ finalEffectiveType }}  &nbsp; </p>
          </div>
        </div>
        <div class="Ineffektiv">
          <div class="ITitle">
            <p style="font-size: large"> Ineffektiv: </p>
          </div>
          <div class="color3"></div> 
          <div class="ITypes">
            <p v-for="(finalWeakType, index) in finalWeakTypes" :key="index"> {{ finalWeakType }} &nbsp; </p>
          </div>               
        </div>
        <div class="SehrIneffektiv">
          <div class="IETitle">
            <p style="font-size: large"> Sehr Ineffektiv: </p>
          </div>
          <div class="color4"></div>  
          <div class="SITypes">
            <p v-for="(veryweakType, index) in veryweakTypes" :key="index"> {{ veryweakType }} &nbsp; </p>
          </div>     
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

    myTypes: [],          // Meine Typen
    enemyTypes: [],       // Gegner Typen
    
    doubleDamageTypes1: [],     // Meine Dmg-Relations von Typ1
    halfDamageTypes1: [],
    noDamageTypes1: [],

    doubleDamageTypes2: [],     // Meine Dmg-Relations von Typ2
    halfDamageTypes2: [],
    noDamageTypes2: [],

    DDTmsg: "",     // Msg falls es keine Dmg-Relations gibt
    HDTmsg: "",
    NDTmsg: "",


    weakTypes: [],          // Array zum zwischenspeichern
    effectiveTypes: [],     // Array zum zwischenspeichern
    
    noRealHalfDmg: [],    // Array zum zwischenspeichern
    almostFinalEffectiveArray: [],  // Array zum zwischenspeichern

    veryeffectiveTypes: [],   // Final Arrays mit richtigen Relations
    finalEffectiveTypes: [],
    finalWeakTypes: [],
    veryweakTypes: [],

   }
 },

methods: {
 
  setTypes(){

    for(let i = 0; i < this.übergebeneTypen.pokemonTypL.length; i++){
      this.myTypes[i] = this.übergebeneTypen.pokemonTypL[i].type.name;
      this.getTypeInfos(this.myTypes[i], i);
    }

    // Wenn ich nur 1 Typ habe (noDamage direkt aus API Call Relation)
    if(this.übergebeneTypen.pokemonTypL.length == 1){
        this.finalEffectiveTypes = this.doubleDamageTypes1;
        this.finalWeakTypes = this.halfDamageTypes1;
    }
    
    for(let i = 0; i < this.übergebeneTypen.pokemonTypR.length; i++){
      this.enemyTypes[i] = this.übergebeneTypen.pokemonTypR[i].type.name;
    }

    setTimeout(() => {
      this.trimTypeRelations();
      this.filterTypesWithNoDmg();
    },500)

  },

  async getTypeInfos(Type, number){
    
    await axios.get(`https://pokeapi.co/api/v2/type/${Type}`).then((response) => {
      let AttackenInformationen = response.data.damage_relations 
      let doubleDamageTo = AttackenInformationen.double_damage_from; 
      let halfDamageTo = AttackenInformationen.half_damage_from; 
      let noDamageTo = AttackenInformationen.no_damage_from;
       
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

    trimTypeRelations(){
       
      let h = 0;
      let k = 0;
      let x = 0;
      let y = 0;
        
      // Wenn Beide Types half dmg gegen ein Typ haben --> veryWeakTypes[]
      for(let i = 0; i < this.halfDamageTypes1.length; i++){
        for(let j = 0; j < this.halfDamageTypes2.length; j++){
          if(this.halfDamageTypes1[i] == this.halfDamageTypes2[j]){
            this.veryweakTypes[k] = this.halfDamageTypes1[i];
            this.noRealHalfDmg[y] = this.halfDamageTypes1[i];
            k++;
            y++;
          }
        }
      }

      // Wenn es keine veryWeakTypes gibt --> "None"
      if(this.veryweakTypes.length == 0){
        this.veryweakTypes[0] = "None";
      }


      // Wenn Beide Types double dmg gegen ein Typ haben --> veryeffectiveTypes[]
      for(let i = 0; i < this.doubleDamageTypes1.length; i++){
        for(let j = 0; j < this.doubleDamageTypes2.length; j++){
          if(this.doubleDamageTypes1[i] == this.doubleDamageTypes2[j]){
            this.veryeffectiveTypes[h] = this.doubleDamageTypes1[i];              
            h++;
          }
        }
      }

      // Wenn es keine veryeffectiveTypes gibt --> "None"
      if(this.veryeffectiveTypes.length == 0){
        this.veryeffectiveTypes[0] = "None";
      }


      // Filter: Nur echte double-dmg-Types von Typ1; wenn einer double einer half --> aussortieren    
      let sameType = false;

      for(let i = 0; i < this.doubleDamageTypes1.length; i++){
        sameType = false;
        for(let j = 0; j < this.halfDamageTypes2.length; j++){
          if(this.doubleDamageTypes1[i] == this.halfDamageTypes2[j]){ //gleiche Typen -> nicht in effective Types aufnehmen
            sameType = true;
            this.noRealHalfDmg[y] = this.halfDamageTypes2[j];    // Typ1 half Dmg & Double Dmg --> noRealHalfDmg
            y++;
          }          
        }
        if(sameType == false){
          this.effectiveTypes[x] = this.doubleDamageTypes1[i];   // Typ1 nur double Dmg & kein half Dmg --> effectiveTypes[]
          x++;
        }
      }


      // Filter: Nur echte double-dmg-Types von Typ2; wenn einer double einer half --> aussortieren
      for(let i = 0; i < this.doubleDamageTypes2.length; i++){
        sameType = false;
        for(let j = 0; j < this.halfDamageTypes1.length; j++){
          if(this.doubleDamageTypes2[i] == this.halfDamageTypes1[j]){
            sameType = true;
            this.noRealHalfDmg[y] = this.halfDamageTypes1[j];   // Typ2 half Dmg & Double Dmg --> noRealHalfDmg
            y++;
          }
        }
        if(sameType == false){
          this.effectiveTypes[x] = this.doubleDamageTypes2[i];  // Typ2 nur double Dmg & kein half Dmg --> effectiveTypes[]         
          x++;
        }
      }


      //Alle Typen die in noRealHalfDmg stehen, sollen nicht mehr in weakTypes[] sein (Durchlauf für Typ1 half Dmg Liste)
      let TypMussRaus = false;
      let q = 0;

      for(let i = 0; i < this.halfDamageTypes1.length; i++){
        TypMussRaus = false;
        for(let j = 0; j < this.noRealHalfDmg.length; j++){
          if(this.halfDamageTypes1[i] == this.noRealHalfDmg[j]){
            TypMussRaus = true;
          }
        }
        if(TypMussRaus == false){
          this.weakTypes[q] = this.halfDamageTypes1[i];
          q++;
        }
      }


      //Alle Typen die in noRealHalfDmg stehen, sollen nicht mehr in weakTypes[] sein (Durchlauf für Typ2 half Dmg Liste)
      for(let i = 0; i < this.halfDamageTypes2.length; i++){
        TypMussRaus = false;
        for(let j = 0; j < this.noRealHalfDmg.length; j++){
          if(this.halfDamageTypes2[i] == this.noRealHalfDmg[j]){
            TypMussRaus = true;
          }
        }
        if(TypMussRaus == false){
          this.weakTypes[q] = this.halfDamageTypes2[i];
          q++;
        }
      }


      //Alle Typen die in veryeffectiveTypes[] stehen, sollen nicht mehr in effectiveTypes[] stehen --> Zwischen Speichern in almostFinalEffectiveArray
      let e = 0;

      for(let i = 0; i < this.effectiveTypes.length; i++){
        TypMussRaus = false;
        for(let j = 0; j < this.veryeffectiveTypes.length; j++){
          if(this.effectiveTypes[i] == this.veryeffectiveTypes[j]){
            TypMussRaus = true;
          }
        }
        if(TypMussRaus == false){
          this.almostFinalEffectiveArray[e] = this.effectiveTypes[i];
          e++;
        }
      }

    },

    
    filterTypesWithNoDmg(){
      // Filter: wenn irgendwo No-Dmg-Type existiert --> Relation aus effective/weak Type Arrays Löschen      
      let arrcount1 = 0;
      let arrcount2 = 0;
      
      for(let j = 0; j < this.almostFinalEffectiveArray.length; j++){
        if(this.noDamageTypes1[0] != this.almostFinalEffectiveArray[j]){
          if(this.noDamageTypes2[0] != this.almostFinalEffectiveArray[j]){
            this.finalEffectiveTypes[arrcount1] = this.almostFinalEffectiveArray[j];
            arrcount1++;
          }
        }
      }
      for(let i = 0; i < this.weakTypes.length; i++){
        if(this.noDamageTypes1[0] != this.weakTypes[i]){
          if(this.noDamageTypes2[0] != this.weakTypes[i]){
            this.finalWeakTypes[arrcount2] = this.weakTypes[i];
            arrcount2++;
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

.ViewSkillset{
  height: 20px;
  width: 100%;
  text-align: center;
  margin-top: 15px;
}

.ViewSkillsetButton{
  height: 22px;
  width: 200px;
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

    .SehrEffektiv{
      width: 100%;
      height: 25%;
      display: flex;
      flex-direction: row;
    }

      .SETitle{
        width: 110px;
        height: 100%;
      }

      .color1{
        height: 15px;
        width: 15px;
        background-color: green;
        border: solid;
        border-width: 2px;
        border-color: green;
        border-radius: 15px;
        margin-top: 20px;
        margin-left: 5px;
      }

      .SETypes{
        display: flex;
        flex-direction: row;
        margin-left: 35px;
        text-transform: capitalize;
        font-size: large;
      }

    .Effektiv{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

      .ETitle{
        width: 110px;
        height: 100%;
      }

      .color2{
        height: 15px;
        width: 15px;
        background-color: yellow;
        border: solid;
        border-width: 2px;
        border-color: yellow;
        border-radius: 15px;
        margin-top: 20px;
        margin-left: 5px;
      }

      .ETypes{
        display: flex;
        flex-direction: row;
        margin-left: 35px;
        text-transform: capitalize;
        font-size: large;
      }

    .Ineffektiv{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

      .ITitle{
        width: 110px;
        height: 100%;
      }

      .color3{
        height: 15px;
        width: 15px;
        background-color: orange;
        border: solid;
        border-width: 2px;
        border-color: orange;
        border-radius: 15px;
        margin-top: 20px;
        margin-left: 5px;
      }

      .ITypes{
        display: flex;
        flex-direction: row;
        margin-left: 35px;
        text-transform: capitalize;
        font-size: large;
      }

    .SehrIneffektiv{
      display: flex;
      flex-direction: row;
      width: 100%;
      height: 25%;
    }

      .SITitle{
        width: 110px;
        height: 100%;
      }

      .color4{
        height: 15px;
        width: 15px;
        background-color: red;
        border: solid;
        border-width: 2px;
        border-color: red;
        border-radius: 15px;
        margin-top: 20px;
        margin-left: 5px;
      }

      .SITypes{
        display: flex;
        flex-direction: row;
        margin-left: 35px;
        text-transform: capitalize;
        font-size: large;
      }

</style>