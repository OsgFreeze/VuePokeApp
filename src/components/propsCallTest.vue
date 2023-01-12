<template>  <!-- ref="divRef"  -->
    <div ref="divRef"> 
      <h1> Hallo Welt -> Props </h1>
      {{this.gespeicherterWert}}
    </div>
  </template>
  
  <script>
      export default {
      name: 'propsCallTest', //Component Name

      data(){  //Variablen [Daten] lokal deklarieren
    return {
      gespeicherterWert: 0, //Variable -> [Type: Number]
    }
  },

     mounted() {
        console.log( this.$refs.divRef.getApiDataMain() ); 
    }    
  }   
  
  </script>
  
  <style>
  </style>