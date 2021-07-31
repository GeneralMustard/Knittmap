<template>
  <div class="map">
    <div class ="row" v-for="(row, rowInd) in cells" :key="rowInd">
      <button
        v-for="(col, colInd) in row" :key="col.id"
        class="cell"
        v-bind:style="{ backgroundColor: '#ABABAB' }"
        v-on:click="updateCell(rowInd, colInd)"
      >{{ getVal(col.val) }}</button>
    </div>
    
  </div>
</template>

<script>

export default {
  name: 'Map',
  props: {
    // Each list in cells is a row
    cells: Array,
    option: Number,
    colNr: Number,
    rowNr: Number
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
    updateCell(rowInd, colInd) {
      // TODO just send in cell
      this.cells[rowInd][colInd].val = this.option;
    },
    getVal(val) {
      if (val === 0) return;
      return val;
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
  margin: 40px;
  overflow-x: auto;
  overflow-y: auto;
}

.cell {
  height: 50px;
  width: 50px;
  min-width: 50px;
  min-height: 50px;
  border: 1px solid rgba(0.2, 0.2, 0.2, 0.2);
  font-size: 20px;
}

.cell:hover {background-color: #ffffff}

.cell:active {
  background-color: #666565;
}

.button {
  padding: 15px 25px;
  font-size: 24px;
  text-align: center;
  cursor: pointer;
  outline: none;
  color: #fff;
  background-color: #04AA6D;
  border: none;
  border-radius: 15px;
  box-shadow: 0 9px #999;
}

.button:hover {background-color: #3e8e41}


</style>
