<template>
  <div>
    <b-button
      block
      variant="outline-primary"
      :pressed.sync="editOpen"
    > {{ editButton }} </b-button>

    <div
      class="row option"
      v-for="opt in options" :key="opt.id"
    >

      <b-button
        variant="outline-secondary"
        :pressed="isActive(opt.id)"
        v-on:click="updateActive(opt.id), $emit('change-option', active)"
      >{{ opt.id }}</b-button>

      <b-img
        v-bind="mainProps(opt.color)"
        rounded alt="Rounded image"
      ></b-img>

      <b-button
        v-if="editOpen"
        variant="danger"
        v-on:click="removeOption(opt.id),
                    $emit('remove-option', opt.id),
                    $emit('change-option', active)"
      >x</b-button>

      <b-form-input
        v-if="editOpen"
        v-model="opt.color"
        placeholder="opt.color"
      ></b-form-input>
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

export default {
  name: 'CellOptions',
  props: {
  },
  components: {
  },
  data() {
    return {
      options: [{ id: 0, color: '#fc0f00' }],
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
    mainProps(color) {
      return { blank: true, blankColor: color, width: 40, height: 40, class: 'color-prev' }
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
      // Could this be prettier?
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
    }
  }
}
</script>

<style scoped>
  .option {
    margin: 10px;
  }
  .color-prev {
    border: 2px solid rgba(0.2, 0.2, 0.2, 0.2);
  }
</style>
