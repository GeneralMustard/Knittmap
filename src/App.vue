<template>
  <div id="app" class="container">
    <h1>Knittmap</h1>
    <div class="row">
      <div class="col-10">
        <Map v-bind:option="this.option"/>
      </div>
      <div class="col">
        <CellOptions v-on:change-option="changeOption"/>
      </div>
    </div>
  </div>
</template>

<script>
import Map from './components/Map.vue'
import CellOptions from './components/CellOptions.vue'

export default {
  name: 'App',
  components: {
    Map,
    CellOptions
  },
  data() {
    return {
      option: null
    }
  },
  methods: {
    changeOption(opt) {
      this.option = opt;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
