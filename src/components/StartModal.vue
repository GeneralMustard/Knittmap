<template>
  <div>
    <b-modal
      id="start-modal"
      hide-footer
      size="sm"
    >
      <b-container>
        <b-col>
          <b-row class="mb-2">
            <b-button
              v-b-modal.new-map
              @click="$bvModal.hide('start-modal')"
              block variant="success"
            >New map</b-button>
          </b-row>

          <b-row>
            <b-button
              v-b-modal.load-file
              @click="$bvModal.hide('start-modal')"
              block
            >Load map</b-button>
          </b-row>
        </b-col>
      </b-container>

    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'StartModal'
}
</script>
