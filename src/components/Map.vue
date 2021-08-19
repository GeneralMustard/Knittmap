<template>
  <div>
    <div
      v-if=!preview
      v-bind:style="{ height: windowHeight}" class="map pb-3"
    >
      <div class ="row" v-for="(row, rowInd) in cells" :key="rowInd">
        <button
          v-for="cell in row" :key="cell.id"
          v-bind:style="{
            backgroundColor: getColor(cell.val),
            width: getZoom(), minWidth: getZoom(),
            height: getZoom(), minheight: getZoom() }"
          class="cell"
          v-on:click="updateCell(cell)"
        />
      </div>
    </div>

    <div
      v-if=preview
      v-bind:style="{ height: windowHeight}" class="map"
    >
      <div class="row" v-for="(row, rowInd) in cells" :key="rowInd">
        <b-img
          v-for="(cell, colInd) in getRows(row)" :key="colInd"
          v-bind="cellProps(getColor(cell.val))"
        />
      </div>
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
    workZoom: Number,
    prevZoom: Number,
    preview: Boolean,
    prevReps: Number
  },
  components: {
  },

  data() {
    return {
      windowHeight: window.innerHeight - 80 + 'px'
    }
  },
  mounted() {
    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize);
    });
  },
  beforeDestroy() { 
    window.removeEventListener('resize', this.onResize); 
  },
  methods: {
    getRows(row) {
      var tmp = [];
      for (let i = 0; i < this.prevReps; i++) {
        tmp.push(...row);
      }
      return tmp;
    },
    cellProps(color) {
      return { blank: true, blankColor: color,
        width: this.getZoom(), minWidth: this.getZoom(),
        height: this.getZoom(), minheight: this.getZoom()} ;
    },
    getZoom() {
      if (this.preview) return this.prevZoom + 'px'
      return this.workZoom + 'px';
    },
    updateCell(cell) {
      cell.val = this.option;
    },
    getColor(val) {
      for (let i = 0; i < this.options.length; i++) {
        if (this.options[i].id === val) {
          return this.options[i].color;
        } 
      }
      return '#FFFFFF';
    },
    onResize() {
      this.windowHeight = window.innerHeight - 80 + 'px'
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
  overflow-x: auto;
  overflow-y: auto;
}

.cell {
  border: 1px solid rgba(0.2, 0.2, 0.2, 0.2);
}
</style>
