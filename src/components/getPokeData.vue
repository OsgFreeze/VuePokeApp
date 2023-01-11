<template>
    <div>
        <ul class="getPokeData">  </ul>
    </div>

</template>
<script>
export default {
    name: 'getPokeData',
    components: {
    },
    methods: {
        async loadApiNumber(pokeNumber){
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
    }
</style>
  