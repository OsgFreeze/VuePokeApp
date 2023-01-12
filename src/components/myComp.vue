<template>  <!-- ref="divRef"  -->
    <div> 
        <input type="text" :value="msg" @input="changeMessage"/>
        {{  eigeneVariable }}
    </div>
    
  </template>
  
  <script>
      export default {
      name: 'myComp', //Component Name   
      props: ['msg'],  
      data() {
        return {
          eigeneVariable: ''
        }
      },
      methods: {
        changeMessage(übergabeParameter) {
          this. eigeneVariable = übergabeParameter.target.value;
        }
      }
  }   
  
  </script>
  
  <style>
  </style>