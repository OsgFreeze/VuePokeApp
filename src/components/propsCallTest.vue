<template>  <!-- ref="divRef"  -->
    <div> 
        <input type="text" ref="myRef" /> 
    </div>
    
  </template>
  
  <script>
      export default {
      name: 'propsCallTest', //Component Name

     mounted() {   
          console.log(this.$refs.postListRef)   
    }    
  }   
  
  </script>
  
  <style>
  </style>