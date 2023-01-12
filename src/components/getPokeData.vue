<template>

    <div id="eingabeFenster">   <!-- [Container]  alle User Eingaben -->
        <input type="number" v-model="NumberInput"> 
        <button @click="getApiData" > Absenden </button>
        &nbsp;
        <button @click="setNrPlus"> Hoch ↑ </button>
        <button @click="setNrMinus"> Runter ↓ </button>
    </div>

    <div v-if="visible"> <!-- [Container]  Pokemon Name Ausgeben/Rendern -->
        {{myPokemonName}} 
    </div>

    
</template>

<script>
import axios from 'axios' //Axios für API CALL importieren

export default {
    name: 'getPokeData', //Component Name

    data(){ 
        return {
            visible: false, //Rendert erst sobald 'positiv' gesetzt wird [Type: boolean]
            myPokemonName: "", //Pokemon Name [Type: String]
            NumberInput: 1, // Startwert = 1 [Type: Number]
        }
    },

    methods: {

        async getApiData(){ //Api Aufruf -> wichtig: API Objekt kann nur hier variablen zugewiesen werden
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.NumberInput}`).then((response) => {
                console.log(response); 

                const data = response.data; 
                this.myPokemonName = data.name; 
                this.visible=true; 
            })
        },
        async setNrPlus() { // Methode für den [Hoch ↑] Button
            this.NumberInput = (this.NumberInput + 1);
            this.getApiData();
        },
        async setNrMinus() { // Methode für den [Runter ↓] Button
            this.NumberInput = (this.NumberInput - 1);
            this.getApiData();
        },
    }
}   
</script>

<style> 
</style>
  