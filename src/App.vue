<template>
  <div id="app">
    <h1>Knittmap</h1>
    <b-container>
      <b-row>
        <b-col cols="10">
          <Map
            v-bind:options="this.options"
            v-bind:option="this.activeOption"
            v-bind:cells="this.cells"
            v-bind:rowNr="this.rowNr"
            v-bind:colNr="this.colNr"
          />
        </b-col>
        <b-col cols="2">
          <CellOptions
            v-bind:activeOption="this.activeOption"
            v-bind:options="this.options"
            v-on:change-active-option="changeActiveOption"
            v-on:remove-option="removeOption"
          />
        </b-col>
      </b-row>
    </b-container>
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
      options: [{ id: 0, color: '#ABABAB' }],
      activeOption: 0,

      cells: [],
      colNr: 50,
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
    // // Change the active option
    changeActiveOption(opt) {
      this.activeOption = opt;
    },
    // Remove an option
    removeOption(id) {
      // remove option with corresponding id from list of options
      for (let i = 0; i < this.options.length; i++) {
        if (id === this.options[i].id) {
          this.options.splice(i, 1);
          if (id === this.activeOption) this.activeOption = 0;
          break;
        }
      }
      // replace all vals with 0 for cells with the removed option
      for (let i = 0; i < this.rowNr; i++) {
        for (let j = 0; j < this.colNr; j++) {
          if (id === this.cells[i][j].val)
            this.cells[i][j].val = 0;
        }
      }
      // change the active option if it was active when removed
      if (this.activeOption === id) this.changeActiveOption(0);
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
