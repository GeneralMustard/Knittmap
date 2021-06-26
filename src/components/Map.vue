<template>
  <div class="map">
    <div class ="row" v-for="(row, rowInd) in cells" :key="rowInd">
      <button
        v-for="(col, colInd) in row" :key="col.id"
        class="cell"
        v-on:click="updateCell(rowInd, colInd)">{{ getVal(col.val) }}</button>
    </div>
    
  </div>
</template>

<script>

export default {
  name: 'Map',
  props: {
    option: Number
  },
  components: {
  },

  data() {
    return {
      // Each list in cells is a row
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
  computed: {},
  methods: {
    updateCell(rowInd, colInd) {
      this.cells[rowInd][colInd].val = this.option;
      this.text = this.cells;
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
  background-color: rgba(212, 209, 209, 0.8);
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
