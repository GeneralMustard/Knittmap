<template>
  <div>
    <b-modal
      id="load-file"
      title="Load knittmap from file"
      size="sm"
      :ok-disabled="this.file == null"
      @ok="$emit('load-file', file)"
      @show="resetModal"
    >
      <b-form-file
        v-model="file"
        :state="Boolean(file)"
        placeholder="Choose or drop file"
        drop-placeholder="Drop file here"
      ></b-form-file>

      <p class="mt-2">Note: will overwrite current map</p>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'LoadFile',
  data() {
    return {
      file: null
    }
  },
  methods: {
    resetModal() {
      this.file = null;
    }
  }
}
</script>