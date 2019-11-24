<template>
  <div id="app">
    <h1>Tres en raya</h1>
    <ModeSelector :eventbus = "bus"/>
    <Tablero :eventbus= "bus"/>
  </div>
</template>

<script>
import Tablero from './components/Tablero.vue'
import ModeSelector from './components/ModeSelector.vue'
import Vue from 'vue'

export default {
  name: 'app',
  data: function() {
    return {bus: null}
  },
  created(){
    this.bus = new Vue();
  },
  components: {
    Tablero,
    ModeSelector
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
