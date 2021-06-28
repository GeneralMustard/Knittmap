<template>
  <div id="app" class="container">
    <h1>Knittmap</h1>
    <div class="row">
      <div class="col-10">
        <Map
          v-bind:option="this.option"
          v-bind:cells="this.cells"
          v-bind:rowNr="this.rowNr"
          v-bind:colNr="this.colNr"
        />
      </div>
      <div class="col">
        <CellOptions
          v-on:change-option="changeOption"
          v-on:remove-option="removeOption"
        />
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
      option: 0,
      cells: [],
      colNr: 10,
      rowNr: 50
    }
  },
  mounted() {
    var tmpId = 0;
    this.colNr 
    for (let i = 0; i < this.rowNr; i++) {
      var tmpRow = [];
      for (let j = 0; j < this.colNr; j++) {
        tmpRow.push({val: 0, id: tmpId});
        tmpId++;
      }
      this.cells.push(tmpRow);
    }
  },
  methods: {
    changeOption(opt) {
      this.option = opt;
    },
    removeOption(val) {
      for (let i = 0; i < this.rowNr; i++) {
        for (let j = 0; j < this.colNr; j++) {
          if (val === this.cells[i][j].val)
            this.cells[i][j].val = 0;
        }
      }
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
