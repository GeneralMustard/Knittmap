<template>
  <div class="map">
    <div class ="row" v-for="(row, rowInd) in cells" :key="rowInd">
      <button
        v-for="cell in row" :key="cell.id"
        v-bind:style="{ backgroundColor: getColor(cell.val), width: getZoom(), height: getZoom() }"
        class="cell"
        v-on:click="updateCell(cell)"
      />
    </div>
    
  </div>
</template>

<script>

export default {
  name: 'Map',
  props: {
    // The different options and their color
    options: Array,
    // Each list in cells is a row
    cells: Array,
    option: Number,
    colNr: Number,
    rowNr: Number,
    showColor: Boolean,
    zoom: Number
  },
  components: {
  },

  data() {
    return {
    }
  },
  mounted() {
  },
  computed: {},
  methods: {
    getZoom() {
      return this.zoom + 'px';
    },
    updateCell(cell) {
      cell.val = this.option;
    },
    getColor(val) {
      // if (!this.showColor) {
      //   if (val === 0) return '#ABABAB';
      //   return '#FFFFFF';
      // }

      for (let i = 0; i < this.options.length; i++) {
        if (this.options[i].id === val) {
          return this.options[i].color;
        } 
      }
      return '#FFFFFF';
    }
  }
}
</script>

<style scoped>
.row {
  margin-left: 0px;
  flex-wrap: nowrap;
}
.map {
  height: 500px;
  overflow-x: auto;
  overflow-y: auto;
}

.cell {
  border: 1px solid rgba(0.2, 0.2, 0.2, 0.2);
}
</style>
