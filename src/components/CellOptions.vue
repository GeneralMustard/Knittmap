<template>
  <div>
    <b-button
      block
      variant="outline-primary"
      :pressed.sync="editOpen"
    > {{ editButton }} </b-button>

    <div
      class="option"
      v-for="opt in options" :key="opt.id"
    >
      <b-row class="justify-content-md-center">
        <b-button
          variant="outline-secondary"
          :pressed="isActive(opt.id)"
          v-on:click="updateActive(opt.id), $emit('change-option', active)"
        >{{ idButtonText(opt.id) }}</b-button>

        <ColorPicker
          v-bind:initColor="opt.color"
          v-bind:id="opt.id"
          v-on:update-color-option="updateColorOption"
        />
      </b-row>

      <b-row>
        <b-button
          v-if="editOpen && (opt.id !== 0)"
          variant="danger"
          v-on:click="removeOption(opt.id),
                      $emit('remove-option', opt.id),
                      $emit('change-option', active)"
        >x</b-button>
      </b-row>
    </div>

    <b-button
      v-if="isAddAvailable()"
      block
      variant="outline-success"
      v-on:click="addOption()"
    >+</b-button>
  </div>
</template>


<script>
import ColorPicker from './ColorPicker.vue'

export default {
  name: 'CellOptions',
  props: {
  },
  components: {
    ColorPicker
  },
  data() {
    return {
      options: [{ id: 0, color: '#ABABAB' }],
      allOptions: [1,2,3,4,5,6,7,8,9],
      active: 0,
      editOpen: false
    }
  },
  mounted() {
  },
  computed: {
    editButton() {
      if (this.editOpen) return "Close";
      return "Edit";
    }
  },
  methods: {
    idButtonText(id) {
      if (id === 0) return 'x';
      return id;
    },
    updateActive(id) {
      this.active = id;
    },
    isActive(id) {
      if (id == this.active) return true;
      return false;
    },
    removeOption(id) {
      for (let i = 0; i < this.options.length; i++) {
        if (id === this.options[i].id) {
          this.options.splice(i, 1);
          if (id === this.active) this.active = 0;
          return;
        }
      }
    },
    isAddAvailable() {
      return this.options.length - 1 !== this.allOptions.length;
    },
    addOption() {
      for (let i = 0; i < this.allOptions.length; i++) {
        var taken = false;
        for (let j = 0; j < this.options.length; j++) {
          if (this.allOptions[i] === this.options[j].id) {
            taken = true;
            break;
          }
        }
        if (!taken) {
          this.options.push({ id: this.allOptions[i], color: '#FFFFFF' });
          break;
        }
      }
    },
    updateColorOption(id, newColor) {
      for (let i = 0; i < this.options.length; i++) {
        if (id == this.options[i].id) {
          this.options[i].color = newColor;
          return;
        }
      }
    }
  }
}
</script>

<style scoped>
  .option {
    margin: 15px;
  }
  .color-prev {
    border: 2px solid rgba(0.2, 0.2, 0.2, 0.2);
  }
</style>
