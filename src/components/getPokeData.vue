<template>

    <a> {{ loadApiNumber() }} </a>
    <div> <displayData :myPokemonName="myPokemonName" />

        <input type="number" v-model="PokeNumber"> 
        <button @click="loadApiNumber"> Button </button>
    </div>

</template>

<script>
import axios from 'axios'

export default {
    name: 'getPokeData',
    components: {
    },
    data(){ 
    return {
      returnData: null,
      visible: false,
      myPokemonName: "",
    }
    },
    methods: {
        async loadApiNumber(){
            await axios.get(`https://pokeapi.co/api/v2/pokemon/1`).then((response) => {

                console.log(response);
                const data = response.data;

                this.myPokemonName = data.name;
                this.visible=true;
    })
    },
    async setNrPlus() {
        this.PokeNumber = (this.PokeNumber + 1);
        this.loadApiNumber();
    },
    async setNrMinus() {
        this.PokeNumber = (this.PokeNumber - 1);
        this.loadApiNumber();
    },
}
}   
</script>

<style>
    .getPokeData{
        list-style: none;
    }
</style>
  