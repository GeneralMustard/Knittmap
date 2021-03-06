<template>
  <div>
    <b-button
      class="mb-1"
      block
      variant="outline-primary"
      :pressed.sync="editOpen"
    > {{ editButton }} </b-button>

    <Container 
      @drop="onCellOptionDrop"
      lock-axis="y"
    >
      <Draggable v-for="opt in options" :key="opt.id">
        <b-container class="mt-2 mb-2">
          <b-row>
            <b-button
              variant="outline-secondary"
              :pressed="isActive(opt.id)"
              v-on:click="$emit('change-active-option', opt.id)"
            >{{ idButtonText(opt.id) }}</b-button>

            <ColorPicker
              v-bind:initColor="opt.color"
              v-bind:id="opt.id"
              v-on:update-color-option="updateColorOption"
            />
          </b-row>

          <b-row>
            <b-button
              class="mt-1"
              v-if="editOpen && (opt.id !== 0)"
              variant="outline-danger"
              block
              v-on:click="$emit('remove-option', opt.id)"
            >-</b-button>
          </b-row>
        </b-container>
      </Draggable>
    </Container>

    <b-button
      class="mt-1"
      v-if="editOpen && isAddAvailable()"
      block
      variant="outline-success"
      v-on:click="addOption"
    >+</b-button>
  </div>
</template>


<script>
import ColorPicker from './ColorPicker.vue'
import { Container, Draggable } from "vue-smooth-dnd";

export default {
  name: 'CellOptions',
  props: {
    options: Array,
    activeOption: Number
  },
  components: {
    ColorPicker,
    Container,
    Draggable
  },
  data() {
    return {
      allOptions: [1,2,3,4,5,6,7,8,9],
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
    isActive(id) {
      if (id == this.activeOption) return true;
      return false;
    },
    isAddAvailable() {
      return this.options.length - 1 !== this.allOptions.length;
    },
    // Add a new option
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
    // Update the color for an option
    updateColorOption(id, newColor) {
      for (let i = 0; i < this.options.length; i++) {
        if (id == this.options[i].id) {
          this.options[i].color = newColor;
          return;
        }
      }
    },
    onCellOptionDrop(dropResult) {
      const { removedIndex, addedIndex, payload } = dropResult;
      if (removedIndex === null && addedIndex === null) return;

      let itemToAdd = payload;

      if (removedIndex !== null) {
        itemToAdd = this.options.splice(removedIndex, 1)[0];
      }

      if (addedIndex !== null) {
        this.options.splice(addedIndex, 0, itemToAdd);
      }
    }
  }
}
</script>

<style scoped>
  .color-prev {
    border: 2px solid rgba(0.2, 0.2, 0.2, 0.2);
  }
</style>
