<template>
  <div> 
    <h1> Hallo Welt </h1>
    <h1> {{this.variable}} </h1>
  </div>
</template>

<script>
    export default {
    name: 'pictureData', //Component Name
    inject: ['variable'],
}   

</script>

<style>
</style>