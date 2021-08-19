<template>
  <div id="app">
    <b-navbar type="dark" variant="dark" class="mb-2">
      <b-navbar-brand>Knittmap</b-navbar-brand>

      <b-navbar-nav class="mr-auto ml-auto">
        <b-button-group size="sm">
          <b-button
            variant="outline-light"
            :disabled="zoomOutAvailable()"
            class="mr-2"
            v-on:click="zoom(-1)"
          >Zoom out</b-button>
          <b-button
            variant="outline-light"
            :disabled="zoomInAvailable()"
            class="mr-2"
            v-on:click="zoom(1)"
          >Zoom in</b-button>
          <b-button
            variant="outline-light"
            :pressed.sync="preview"
          >Preview</b-button>
        </b-button-group>
      </b-navbar-nav>

      <b-form-spinbutton
        v-if=preview
        v-model="prevReps"
        min="1" max="10"
        size="sm"
        inline
      />

      <b-navbar-nav class="ml-auto">
        <b-button-group size="sm">
          <b-button
            variant="light" class="mr-2" v-on:click="initMap"
          >New</b-button>
          <b-button
            variant="success" class="mr-2" v-on:click="saveFile"
          >Save</b-button>
          <b-button
            class="mr-2"
            v-b-modal.load-file
          >Load</b-button>
        </b-button-group>
      </b-navbar-nav>
    </b-navbar>

    <LoadFile v-on:load-file="loadFile"/>

    <b-container>
      <b-row>
        <b-col>
          <Map
            v-bind:options="this.options"
            v-bind:option="this.activeOption"
            v-bind:cells="this.cells"
            v-bind:rowNr="this.rowNr"
            v-bind:colNr="this.colNr"
            v-bind:workZoom="this.workZoom"
            v-bind:prevZoom="this.prevZoom"
            v-bind:preview="this.preview"
            v-bind:prevReps="this.prevReps"
          />
        </b-col>
        <b-col v-if=!preview cols="2">
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

import LoadFile from './components/LoadFile'
import FileSaver from 'file-saver';

export default {
  name: 'App',
  components: {
    Map,
    CellOptions,
    LoadFile
  },
  data() {
    return {
      options: [{ id: 0, color: '#ABABAB' }],
      cells: [],

      activeOption: 0,

      workZoom: 20,
      prevZoom: 10,

      preview: false,
      prevReps: 3
    }
  },
  computed: {
    rowNr() {
      return this.cells.length;
    },
    colNr() {
      if (this.cells[0]) return this.cells[0].length;
      return 0;
    }
  },
  mounted() {
    this.initMap();
  },
  methods: {
    // Fill the map with empty cells
    initMap() {
      this.cells = []; // reset cells
      var rowNr = 55;  // initial value
      var colNr = 8;  // initial value

      var tmpId = 0;
      for (let i = 0; i < rowNr; i++) {
        var tmpRow = [];
        for (let j = 0; j < colNr; j++) {
          tmpRow.push({val: 0, id: tmpId});
          tmpId++;
        }
        this.cells.push(tmpRow);
      }
    },
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
      // change the active option if it was active when removed
      if (this.activeOption === id) this.changeActiveOption(0);
    },
    // Save the active knittmap to a json-file
    saveFile() {
      var data = { options: this.options, cells: this.cells };
      var blob = new Blob([JSON.stringify(data)]);
      FileSaver.saveAs(blob, "knittmap.json");
    },
    // Load a file and render the knittmap from this
    loadFile(file) {
      let reader = new FileReader();
      reader.onload = e => {
        var data = JSON.parse(e.target.result);
        this.options = data.options;
        this.cells = data.cells;
      };
      reader.readAsText(file);
    },
    zoom(n) {
      if (this.preview) this.prevZoom += (5 * n);
      else this.workZoom += (5 * n);
    },
    zoomOutAvailable() {
      if (this.preview) return this.prevZoom <= 10;
      else return this.workZoom <= 10;
    },
    zoomInAvailable() {
      if (this.preview) return this.prevZoom >= 55;
      else return this.workZoom >= 55;
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
  color: #ffffff;
}
</style>
