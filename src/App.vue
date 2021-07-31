<template>
  <div id="app">
    <h1>Knittmap</h1>
    <b-container>
      <b-row>
        <b-col cols="10">
          <Map
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
            v-bind:allOptions="this.allOptions"
            v-on:change-active-option="changeActiveOption"
            v-on:remove-option="removeOption"
            v-on:add-option="addOption"
            v-on:update-color-option="updateColorOption"
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
      allOptions: [1,2,3,4,5,6,7,8,9],
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
    // Change the active option
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
      if (this.activeOption === id) this.changeActiveOption(0);
    },
    // Add a new option
    addOption() {
      for (let i = 0; i < this.allOptions.length; i++) {
        var taken = false;
        for (let j = 0; j < this.options.length; j++) {
          if (this.allOptions[i] === this.options[j].id) {
            taken = true;
            break;
          }
        }
        if (!taken) {
          this.options.push({ id: this.allOptions[i], color: '#FFFFFF' });
          break;
        }
      }
    },
    // Update the color for an option
    updateColorOption(id, newColor) {
      for (let i = 0; i < this.options.length; i++) {
        if (id == this.options[i].id) {
          this.options[i].color = newColor;
          return;
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
