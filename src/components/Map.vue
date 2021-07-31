<template>
  <div class="map">
    <div class ="row" v-for="(row, rowInd) in cells" :key="rowInd">
      <button
        v-for="cell in row" :key="cell.id"
        v-bind:style="{ backgroundColor: getColor(cell.val) }"
        class="cell"
        v-on:click="updateCell(cell)"
      >{{ getVal(cell.val) }}</button>
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
    showColor: Boolean
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
    updateCell(cell) {
      cell.val = this.option;
    },
    getVal(val) {
      if (val === 0) return;
      return val;
    },
    getColor(val) {
      if (!this.showColor) {
        if (val === 0) return '#ABABAB';
        return '#FFFFFF';
      }

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
  margin: 40px;
  height: 500px;
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
