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
            v-b-modal.new-map variant="light"
          >New</b-button>
          <b-button
            class="ml-2 mr-2"
            v-b-modal.load-file
          >Load</b-button>
          <b-button
            variant="success"
            v-on:click="saveFile" :disabled="cells.length < 1"
          >Save</b-button>
          <b-button
            variant="info" class="ml-2 mr-2"
            v-b-toggle.sb-info
          >?</b-button>
        </b-button-group>
      </b-navbar-nav>
    </b-navbar>

    <NewMap   v-on:new-map="initMap"/>
    <LoadFile v-on:load-file="loadFile"/>
    <Info     v-on:load-example="loadExample"/>

    <b-container>
      <b-row v-if="cells.length > 0" >
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
        <b-col v-if="!(preview||(options.length === 0))" cols="2">
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
import Info from './components/Info.vue'

import NewMap from './components/NewMap'
import LoadFile from './components/LoadFile'
import FileSaver from 'file-saver';

import {v4 as uuidv4} from 'uuid';

export default {
  name: 'App',
  components: {
    Map,
    CellOptions,
    NewMap,
    LoadFile,
    Info
  },
  data() {
    return {
      options: [],
      cells: [],

      activeOption: 0,

      workZoom: 20,
      prevZoom: 10,

      preview: false,
      prevReps: 3,

      savedData: '',
      example_1: {"options":[{"id":0,"color":"#FFFFFF"},{"id":1,"color":"#AFAFAF"},{"id":2,"color":"#C21338"},{"id":3,"color":"#FC6F74"},{"id":4,"color":"#7A031F"},{"id":5,"color":"#A10D2C"}],"cells":[[{"val":0,"id":0},{"val":0,"id":1},{"val":1,"id":2},{"val":1,"id":3},{"val":1,"id":4},{"val":0,"id":5},{"val":0,"id":6},{"val":0,"id":7}],[{"val":0,"id":8},{"val":0,"id":9},{"val":2,"id":10},{"val":2,"id":11},{"val":2,"id":12},{"val":0,"id":13},{"val":0,"id":14},{"val":0,"id":15}],[{"val":0,"id":16},{"val":0,"id":17},{"val":2,"id":18},{"val":2,"id":19},{"val":2,"id":20},{"val":0,"id":21},{"val":0,"id":22},{"val":0,"id":23}],[{"val":0,"id":24},{"val":0,"id":25},{"val":1,"id":26},{"val":2,"id":27},{"val":1,"id":28},{"val":0,"id":29},{"val":0,"id":30},{"val":0,"id":31}],[{"val":0,"id":32},{"val":0,"id":33},{"val":1,"id":34},{"val":2,"id":35},{"val":1,"id":36},{"val":1,"id":37},{"val":0,"id":38},{"val":0,"id":39}],[{"val":0,"id":40},{"val":0,"id":41},{"val":1,"id":42},{"val":2,"id":43},{"val":1,"id":44},{"val":1,"id":45},{"val":0,"id":46},{"val":0,"id":47}],[{"val":0,"id":48},{"val":0,"id":49},{"val":1,"id":50},{"val":2,"id":51},{"val":1,"id":52},{"val":1,"id":53},{"val":0,"id":54},{"val":0,"id":55}],[{"val":0,"id":56},{"val":0,"id":57},{"val":2,"id":58},{"val":2,"id":59},{"val":2,"id":60},{"val":1,"id":61},{"val":0,"id":62},{"val":0,"id":63}],[{"val":0,"id":64},{"val":2,"id":65},{"val":2,"id":66},{"val":4,"id":67},{"val":2,"id":68},{"val":2,"id":69},{"val":0,"id":70},{"val":0,"id":71}],[{"val":0,"id":72},{"val":2,"id":73},{"val":4,"id":74},{"val":4,"id":75},{"val":4,"id":76},{"val":2,"id":77},{"val":0,"id":78},{"val":0,"id":79}],[{"val":0,"id":80},{"val":2,"id":81},{"val":2,"id":82},{"val":4,"id":83},{"val":2,"id":84},{"val":2,"id":85},{"val":0,"id":86},{"val":0,"id":87}],[{"val":0,"id":88},{"val":2,"id":89},{"val":2,"id":90},{"val":2,"id":91},{"val":2,"id":92},{"val":2,"id":93},{"val":0,"id":94},{"val":0,"id":95}],[{"val":0,"id":96},{"val":3,"id":97},{"val":2,"id":98},{"val":2,"id":99},{"val":2,"id":100},{"val":3,"id":101},{"val":0,"id":102},{"val":0,"id":103}],[{"val":0,"id":104},{"val":3,"id":105},{"val":2,"id":106},{"val":2,"id":107},{"val":2,"id":108},{"val":3,"id":109},{"val":0,"id":110},{"val":0,"id":111}],[{"val":0,"id":112},{"val":3,"id":113},{"val":3,"id":114},{"val":2,"id":115},{"val":3,"id":116},{"val":3,"id":117},{"val":0,"id":118},{"val":0,"id":119}],[{"val":0,"id":120},{"val":3,"id":121},{"val":3,"id":122},{"val":2,"id":123},{"val":3,"id":124},{"val":3,"id":125},{"val":3,"id":126},{"val":0,"id":127}],[{"val":0,"id":128},{"val":2,"id":129},{"val":3,"id":130},{"val":3,"id":131},{"val":3,"id":132},{"val":2,"id":133},{"val":3,"id":134},{"val":0,"id":135}],[{"val":0,"id":136},{"val":2,"id":137},{"val":2,"id":138},{"val":3,"id":139},{"val":2,"id":140},{"val":2,"id":141},{"val":3,"id":142},{"val":0,"id":143}],[{"val":0,"id":144},{"val":2,"id":145},{"val":2,"id":146},{"val":3,"id":147},{"val":2,"id":148},{"val":2,"id":149},{"val":3,"id":150},{"val":0,"id":151}],[{"val":0,"id":152},{"val":3,"id":153},{"val":2,"id":154},{"val":2,"id":155},{"val":2,"id":156},{"val":3,"id":157},{"val":3,"id":158},{"val":0,"id":159}],[{"val":0,"id":160},{"val":3,"id":161},{"val":3,"id":162},{"val":2,"id":163},{"val":3,"id":164},{"val":3,"id":165},{"val":3,"id":166},{"val":0,"id":167}],[{"val":0,"id":168},{"val":3,"id":169},{"val":2,"id":170},{"val":2,"id":171},{"val":2,"id":172},{"val":3,"id":173},{"val":3,"id":174},{"val":0,"id":175}],[{"val":0,"id":176},{"val":2,"id":177},{"val":2,"id":178},{"val":1,"id":179},{"val":2,"id":180},{"val":2,"id":181},{"val":3,"id":182},{"val":0,"id":183}],[{"val":2,"id":184},{"val":2,"id":185},{"val":1,"id":186},{"val":1,"id":187},{"val":1,"id":188},{"val":2,"id":189},{"val":2,"id":190},{"val":0,"id":191}],[{"val":2,"id":192},{"val":1,"id":193},{"val":1,"id":194},{"val":4,"id":195},{"val":1,"id":196},{"val":1,"id":197},{"val":2,"id":198},{"val":0,"id":199}],[{"val":2,"id":200},{"val":1,"id":201},{"val":4,"id":202},{"val":4,"id":203},{"val":4,"id":204},{"val":1,"id":205},{"val":2,"id":206},{"val":0,"id":207}],[{"val":2,"id":208},{"val":1,"id":209},{"val":1,"id":210},{"val":4,"id":211},{"val":1,"id":212},{"val":1,"id":213},{"val":2,"id":214},{"val":0,"id":215}],[{"val":2,"id":216},{"val":2,"id":217},{"val":1,"id":218},{"val":1,"id":219},{"val":1,"id":220},{"val":2,"id":221},{"val":2,"id":222},{"val":0,"id":223}],[{"val":2,"id":224},{"val":2,"id":225},{"val":2,"id":226},{"val":1,"id":227},{"val":2,"id":228},{"val":2,"id":229},{"val":2,"id":230},{"val":0,"id":231}],[{"val":3,"id":232},{"val":2,"id":233},{"val":2,"id":234},{"val":2,"id":235},{"val":2,"id":236},{"val":2,"id":237},{"val":3,"id":238},{"val":0,"id":239}],[{"val":3,"id":240},{"val":3,"id":241},{"val":2,"id":242},{"val":2,"id":243},{"val":2,"id":244},{"val":3,"id":245},{"val":3,"id":246},{"val":0,"id":247}],[{"val":3,"id":248},{"val":3,"id":249},{"val":2,"id":250},{"val":2,"id":251},{"val":2,"id":252},{"val":3,"id":253},{"val":3,"id":254},{"val":0,"id":255}],[{"val":3,"id":256},{"val":3,"id":257},{"val":3,"id":258},{"val":2,"id":259},{"val":3,"id":260},{"val":3,"id":261},{"val":3,"id":262},{"val":3,"id":263}],[{"val":3,"id":264},{"val":3,"id":265},{"val":3,"id":266},{"val":2,"id":267},{"val":3,"id":268},{"val":3,"id":269},{"val":3,"id":270},{"val":3,"id":271}],[{"val":2,"id":272},{"val":3,"id":273},{"val":3,"id":274},{"val":3,"id":275},{"val":3,"id":276},{"val":3,"id":277},{"val":2,"id":278},{"val":3,"id":279}],[{"val":2,"id":280},{"val":2,"id":281},{"val":3,"id":282},{"val":3,"id":283},{"val":3,"id":284},{"val":2,"id":285},{"val":2,"id":286},{"val":3,"id":287}],[{"val":2,"id":288},{"val":2,"id":289},{"val":2,"id":290},{"val":3,"id":291},{"val":2,"id":292},{"val":2,"id":293},{"val":2,"id":294},{"val":3,"id":295}],[{"val":2,"id":296},{"val":2,"id":297},{"val":2,"id":298},{"val":3,"id":299},{"val":2,"id":300},{"val":2,"id":301},{"val":2,"id":302},{"val":3,"id":303}],[{"val":4,"id":304},{"val":2,"id":305},{"val":2,"id":306},{"val":3,"id":307},{"val":2,"id":308},{"val":2,"id":309},{"val":4,"id":310},{"val":2,"id":311}],[{"val":4,"id":312},{"val":4,"id":313},{"val":2,"id":314},{"val":2,"id":315},{"val":2,"id":316},{"val":4,"id":317},{"val":4,"id":318},{"val":2,"id":319}],[{"val":2,"id":320},{"val":4,"id":321},{"val":4,"id":322},{"val":2,"id":323},{"val":4,"id":324},{"val":4,"id":325},{"val":2,"id":326},{"val":2,"id":327}],[{"val":2,"id":328},{"val":4,"id":329},{"val":4,"id":330},{"val":2,"id":331},{"val":4,"id":332},{"val":4,"id":333},{"val":2,"id":334},{"val":2,"id":335}],[{"val":2,"id":336},{"val":2,"id":337},{"val":4,"id":338},{"val":4,"id":339},{"val":4,"id":340},{"val":2,"id":341},{"val":2,"id":342},{"val":1,"id":343}],[{"val":1,"id":344},{"val":2,"id":345},{"val":2,"id":346},{"val":4,"id":347},{"val":2,"id":348},{"val":2,"id":349},{"val":1,"id":350},{"val":1,"id":351}],[{"val":1,"id":352},{"val":2,"id":353},{"val":2,"id":354},{"val":2,"id":355},{"val":2,"id":356},{"val":2,"id":357},{"val":1,"id":358},{"val":1,"id":359}],[{"val":1,"id":360},{"val":1,"id":361},{"val":2,"id":362},{"val":2,"id":363},{"val":2,"id":364},{"val":1,"id":365},{"val":1,"id":366},{"val":1,"id":367}],[{"val":1,"id":368},{"val":1,"id":369},{"val":2,"id":370},{"val":2,"id":371},{"val":2,"id":372},{"val":1,"id":373},{"val":1,"id":374},{"val":2,"id":375}],[{"val":2,"id":376},{"val":1,"id":377},{"val":1,"id":378},{"val":2,"id":379},{"val":1,"id":380},{"val":1,"id":381},{"val":2,"id":382},{"val":2,"id":383}],[{"val":1,"id":384},{"val":1,"id":385},{"val":1,"id":386},{"val":2,"id":387},{"val":1,"id":388},{"val":1,"id":389},{"val":1,"id":390},{"val":2,"id":391}],[{"val":1,"id":392},{"val":1,"id":393},{"val":1,"id":394},{"val":1,"id":395},{"val":1,"id":396},{"val":1,"id":397},{"val":1,"id":398},{"val":1,"id":399}],[{"val":1,"id":400},{"val":1,"id":401},{"val":1,"id":402},{"val":1,"id":403},{"val":1,"id":404},{"val":1,"id":405},{"val":1,"id":406},{"val":1,"id":407}],[{"val":1,"id":408},{"val":1,"id":409},{"val":1,"id":410},{"val":1,"id":411},{"val":1,"id":412},{"val":1,"id":413},{"val":1,"id":414},{"val":1,"id":415}],[{"val":1,"id":416},{"val":1,"id":417},{"val":1,"id":418},{"val":1,"id":419},{"val":1,"id":420},{"val":1,"id":421},{"val":1,"id":422},{"val":1,"id":423}],[{"val":1,"id":424},{"val":1,"id":425},{"val":1,"id":426},{"val":1,"id":427},{"val":1,"id":428},{"val":1,"id":429},{"val":1,"id":430},{"val":1,"id":431}],[{"val":1,"id":432},{"val":1,"id":433},{"val":1,"id":434},{"val":1,"id":435},{"val":1,"id":436},{"val":1,"id":437},{"val":1,"id":438},{"val":1,"id":439}]]}
      
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
    this.savedData = this.dataAsString();
    window.addEventListener('beforeunload', this.onBeforeUnload);
    this.$bvModal.show('new-map');
  },

  methods: {
    initMap(rowNr, colNr) {
      this.options = [{ id: 0, color: '#ABABAB' }];
      this.cells = [];
      
      for (let i = 0; i < rowNr; i++) {
        var tmpRow = [];
        for (let j = 0; j < colNr; j++) {
          tmpRow.push({val: 0, id: uuidv4()});
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
      var data = this.dataAsString();
      var blob = new Blob([data]);
      FileSaver.saveAs(blob, "knittmap.json");

      this.savedData = data;
    },
    // Load a file and render the knittmap from this
    loadFile(file) {
      let reader = new FileReader();
      reader.onload = e => {
        var data = JSON.parse(e.target.result);
        this.options = data.options;
        this.cells = data.cells;
        this.savedData = this.dataAsString();
      };
      reader.readAsText(file);
    },
    loadExample() {
      this.cells = JSON.parse(JSON.stringify(this.example_1.cells));
      this.options = JSON.parse(JSON.stringify(this.example_1.options));
    },
    dataAsString() {
      return JSON.stringify({ options: this.options, cells: this.cells });
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
    },
    // Ask for confirmation before unload.
    onBeforeUnload(event) {
      var currentData = this.dataAsString();
      if (currentData != this.savedData) event.preventDefault();
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
  color: #000000;
}
</style>
