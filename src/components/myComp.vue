<template>  <!-- ref="divRef"  -->
    <div> 

      {{ übergebeneVariable }}

      
      <input type="text" @input="changeVariable" /> <!-- :value="übergebeneVariable" -->
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