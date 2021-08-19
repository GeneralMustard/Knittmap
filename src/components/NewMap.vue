<template>
  <div>
    <b-modal
      id="new-map"
      title="New Map"
      @ok="$emit('new-map', rows, cols)"
    >
      <b-container>
        <b-row class="mb-2">
          <b-col>
            <label for="sb-rows">Rows:</label>
            <b-form-spinbutton
              id="sb-rows"
              v-model="rows"
              min="1"
              max="300"
            />
          </b-col>

          <b-col>
            <label for="sb-cols"> Columns:</label>
            <b-form-spinbutton
              id="sb-cols"
              v-model="cols"
              min="1"
              max="300"
            />
          </b-col>
        </b-row>
        <p>Note: will overwrite current map</p>
      </b-container>

    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'NewMap',
  data() {
    return {
      rows: 50,
      cols: 10
    }
  }
}
</script>