<template>  <!-- ref="divRef"  -->
    <div> 
        <input type="text" ref="myRef"/>
    </div>
    
  </template>
  
  <script>
      export default {
      name: 'myComp', //Component Name

     mounted() {   
          console.log(this.$refs.myRef.focus()); 
    }    
  }   
  
  </script>
  
  <style>
  </style>