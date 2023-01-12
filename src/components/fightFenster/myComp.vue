<template>  <!-- ref="divRef"  -->
    <div> 
      {{ übergebeneVariable }} <!-- gib Parent Variable aus -->  <!-- :value="übergebeneVariable" @input="changeVariable -->

      <button  :value="übergebeneVariable" @click="changeVariable" > Scheiß </button>
      {{ this.eigeneVariable }}
    </div>
    
  </template>
  
  <script>
      export default {
      name: 'myComp', //Component Name   
      props: ['übergebeneVariable'],  
      data() {
        return {
          eigeneVariable: "child"
        }
      },
      methods: {
        changeVariable(übergabeParameter) {  
          this.eigeneVariable = übergabeParameter.target.value 
          this.$emit('messageChanged', this.eigeneVariable); //geänderte Zahl zurück schiem     
      },
   } 
  }

  </script>
  
  <style>
  </style>