<template> 

  <myComp :übergebeneVariable="Variable" @messageChanged="Variable = $event" />

</template>

<script> 

import myComp from './components/myComp.vue';

export default {
  name: 'App',
  components: { //Komponenten deklarieren
    myComp
  }, 
  data(){  //Variablen [Daten] lokal deklarieren
    return {
      Variable: "parent"
    }
  },
}
</script>

<style>
</style>

