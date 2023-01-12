<template>  <!-- ref="divRef"  -->
    <div> 
        <button ref="myRef"> Press me </button>
    </div>
    
  </template>
  
  <script>
      export default {
      name: 'myComp', //Component Name

     mounted() {   
          console.log(this.$refs.myRef.printMethod())   
    }    
  }   
  
  </script>
  
  <style>
  </style>