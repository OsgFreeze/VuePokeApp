<template>
    <div class="getPokeData">
        <button @click="loadApiNumber">Click..</button>
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
      PokeNumber: 1,
    }
    },
    methods: {
        async loadApiNumber(){
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${this.PokeNumber}`).then((response) => {
                console.log(response);
                const data = response.data;
                return data;
  })
},
}
}   
</script>

<style>
    .getPokeData{
        list-style: none;
        width: 500px;
    }
</style>
  