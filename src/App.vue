<template>   
  <myComp/>
</template>

<script> 

import myComp from './components/myComp.vue';

export default {
  name: 'App',
  components: { //Komponenten deklarieren
    myComp
  }, 
  methods: {
    printMethod: function () {
      console.log("Hallo");
    },
} 
}
</script>

<style>
</style>

