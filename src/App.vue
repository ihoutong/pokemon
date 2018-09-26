<template>
  <div id="app">
    <Chart style="width: 50%; display: inline-block;" v-on:showPokemons="showPokemons"/>
    <Pokemon style="width: 50%;  display: inline-block;" :names="listData.names" :type="listData.type"/>
  </div>
</template>

<script>
import Chart from './components/Chart.vue'
import Pokemon from './components/Pokemon.vue'

export default {
  name: 'app',
  components: {
    Chart,
    Pokemon
  },
  data() {
    return {
      listData: {}
    }
  },
  methods: {
    showPokemons(data) {
      this.listData = data;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
  width: 90%;
}
</style>
