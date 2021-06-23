<template>
  <div>
    <p id="name">{{ this.text }}</p>
    <div v-for="(row, rowInd) in cells" :key="rowInd">
      <button
        v-for="(col, colInd) in row" :key="col.id"
        class="grid-item"
        v-on:click="updateCell(rowInd, colInd)">{{ col.val }}</button>
    </div>
    
  </div>
</template>

<script>

export default {
  name: 'Map',
  props: {
  },
  components: {
  },

  data() {
    return {
      text: "hello saga",
      // Each list is a row
      cells: [],
      colNr: 5,
      rowNr: 5,
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
      this.cells[rowInd][colInd].val = 1;
      this.text = this.cells;
    }
  }
}
</script>

<style scoped>
.grid-container {
  display: inline-grid;
  grid-template-columns: repeat(2, 1fr);
  background-color: #2196F3;
  padding: 10px;
}

.grid-item {
  background-color: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 30px;
  text-align: center;
}

.grid-item:hover {background-color: #3e8e41}

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

.button:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
</style>
