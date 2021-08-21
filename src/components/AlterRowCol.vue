<template>
  <div>
    <b-modal
      id="alter-row-col"
      :title="title"
      hide-footer
    >
      <b-container>
        <b-row class="mb-3" align-h="between">
          <p>Add copies</p>
          <b-form-spinbutton
            v-model="addNum"
            min="1" max="10"
            size="sm"
            inline
          />

          <b-button
            variant="success"
            size="sm"
            @click="$emit('alter-add', addNum),
              $bvModal.hide('alter-row-col')"
          >Confirm add</b-button>
        </b-row>

        <b-row align-h="between">
          <p>{{ deleteMsg }}</p>
          <b-button
            :disabled="!this.alterProps.canDel"
            variant="danger"
            size="sm"
            @click="$emit('alter-delete'),
              $bvModal.hide('alter-row-col')"
          >Confirm delete</b-button>
        </b-row>
      </b-container>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'AlterRowCol',
  props: {
    alterProps: { ind: Number, name: Number, isRow: Boolean, canDel: Boolean }
  },
  data() {
    return {
      addNum: 1 // The number of copies to add
    }
  },
  computed: {
    title() {
      var tmp = this.alterProps.name;
      if (this.alterProps.isRow) return 'Options for row ' + tmp;
      return 'Options for column ' + tmp;
    },
    deleteMsg() {
      if (this.alterProps.isRow) return 'Delete row'
      return 'Delete column'
    }
  }
}
</script>
