<template> 

  <myComp :msg="Variable" />

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
      Variable: 'Hurensohn'
    }
  },
}
</script>

<style>
</style>

