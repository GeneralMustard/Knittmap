<template>
  <div>
    <div
      v-if=!preview
      v-bind:style="{ height: windowHeight }" class="map pb-3"
    >
      <div class="row mb-1">
        <button class="mr-1"
          v-bind="altRowColProps()"/>
        <button
          v-for="altCol in colNr" :key="altCol"
          v-bind="altRowColProps()"
          @click="alterColModal(altCol - 1)"
        >{{ colNr - (altCol - 1) }}</button>
      </div>

      <div class ="row" v-for="(row, rowInd) in cells" :key="rowInd">
        <button class="mr-1"
          v-bind="altRowColProps()"
          @click="alterRowModal(rowInd)"
        >{{ rowNr - rowInd }}</button>
        <button
          v-for="cell in row" :key="cell.id"
          v-bind:style="{
            backgroundColor: getColor(cell.val),
            width: getZoom(), minWidth: getZoom(),
            height: getZoom(), minheight: getZoom() }"
          class="cell"
          @mousedown="mouseDown = true, updateCell(cell)"
          @mouseup="mouseDown = false"
          @mouseover="updateCell(cell)"
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

    <AlterRowCol
      v-bind:alterProps = "this.alterProps"
      v-on:alter-add="alterAdd"
      v-on:alter-delete="alterDelete"
    />
  </div>
</template>

<script>
import AlterRowCol from './AlterRowCol.vue'

import { v4 as uuidv4 } from 'uuid';

export default {
  name: 'Map',
  props: {
    // The cells as a list of lists where each list represents a row
    cells: Array,
    colNr: Number, // The number of columns
    rowNr: Number, // The number of rows

    options: Array, // The options and their color
    option: Number, // The currently chosen option

    workZoom: Number, // Chosen zoom for work-mode
    prevZoom: Number, // Chosen zoom for preview-mode

    preview: Boolean, // Is preview-mode active
    prevReps: Number  // How many repititions should be shown i preview-mode
  },
  components: {
    AlterRowCol
  },

  data() {
    return {
      windowHeight: window.innerHeight - 80 + 'px',
      // The currently chosen row/col
      alterProps: { ind: null, name: null, isRow: null, canDel: null },
      mouseDown: false
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
    // Returns a list with copies of the given row,
    // the number of copies correstonds to the chosen number of repititions.
    getRows(row) {
      var tmp = [];
      for (let i = 0; i < this.prevReps; i++) tmp.push(...row);
      return tmp;
    },
    // Properties for buttons controlling alternatives
    // for rows and cols.
    altRowColProps() {
      return {
        style: {
          width: this.getZoom(), minWidth: this.getZoom(),
          height: this.getZoom(), minheight: this.getZoom(),
          fontSize: this.getFontZoom()
        },
        class: 'cell'
      };
    },
    // Properties for the cells.
    cellProps(color) {
      return { blank: true, blankColor: color,
        width: this.getZoom(), minWidth: this.getZoom(),
        height: this.getZoom(), minheight: this.getZoom()} ;
    },
    // Returns the current zoom in px as a string.
    getZoom() {
      if (this.preview) return this.prevZoom + 'px'
      return this.workZoom + 'px';
    },
    // Get the font size in px as a string.
    getFontZoom() {
      var tmp = this.workZoom * 0.3
      if (tmp < 4) return '0px'
      return tmp + 'px';
    },
    // Update the value of the given cell to match the currently active option.
    updateCell(cell) {
      if (this.mouseDown) cell.val = this.option;
    },
    // Get the color for the given value.
    getColor(val) {
      for (let i = 0; i < this.options.length; i++)
        if (this.options[i].id === val) return this.options[i].color;

      return '#FFFFFF';
    },
    // Open modal for altering the number of columns.
    alterColModal(ind) {
      this.alterProps = {
        ind: ind, name: this.colNr - ind, isRow: false , canDel: this.colNr > 1
      };
      this.$bvModal.show('alter-row-col');
    },
    // Open modal for altering the number of rows.
    alterRowModal(ind) {
      this.alterProps = {
        ind: ind, name: this.rowNr - ind, isRow: true, canDel: this.rowNr > 1
      };
      this.$bvModal.show('alter-row-col');
    },
    // Add the given number of copies of the currently chosen row/col.
    alterAdd(addNum) {
      var ind = this.alterProps.ind;
      var isRow = this.alterProps.isRow;
      if (ind === null || isRow === null) return;

      if (isRow) {
        var template1 = this.cells[ind];
        // Repeat for each copy
        for (let j = 0; j < addNum; j++) {
          var newRow = [];
          // Create a new row containing the same colors as the template
          for (let i = 0; i < template1.length; i++) {
            newRow.push({ val: template1[i].val, id: uuidv4()});
          }
          // Insert the new row after the template
          this.cells.splice(ind, 0, newRow);
        }
      } else {
        // Repeat for each copy
        for (let j = 0; j < addNum; j++) {
          // Insert a new cell (with same color as the template)
          // in each row, after the template
          for (let i = 0; i < this.cells.length; i++) {
            var template2 = this.cells[i][ind].val;
            this.cells[i].splice(ind, 0, { val: template2, id: uuidv4() });
          }
        }
      }
    },
    // Delete the currently chosen row/col.
    alterDelete() {
      var ind = this.alterProps.ind;
      var isRow = this.alterProps.isRow;
      if (ind === null || isRow === null) return;

      if (isRow) {
        this.cells.splice(ind, 1);
      } else {
        for (let i = 0; i < this.cells.length; i++)
          this.cells[i].splice(ind, 1);
      }
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
