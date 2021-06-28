<template>
  <div>
    <b-button v-b-toggle.OptionsSidebar>Edit Options</b-button>

    <b-sidebar id="OptionsSidebar" title="Options" right>
      <div
        class="row option"
        v-for="opt in options" :key="opt.id"
      >
        <b-button
          variant="outline-primary"
          :pressed="isActive(opt.id)"
          v-on:click="updateActive(opt.id), $emit('change-option', active)"
        >{{ opt.id }}</b-button>
        <b-img v-bind="mainProps(opt.color)" rounded alt="Rounded image"></b-img>
        <b-form-input v-model="opt.color" placeholder="opt.color"></b-form-input>
      </div>
    </b-sidebar>
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
      options: [
          {id: 0, color: '#fc0f00'}
        , {id: 1, color: '#FFFFFF'}
        , {id: 2, color: '#FFFFFF'}
        , {id: 3, color: '#FFFFFF'}
        , {id: 4, color: '#FFFFFF'}
        , {id: 5, color: '#FFFFFF'}
        , {id: 6, color: '#FFFFFF'}
        , {id: 7, color: '#FFFFFF'}
        , {id: 8, color: '#FFFFFF'}
        , {id: 9, color: '#FFFFFF'}
      ],
      active: 0
    }
  },
  mounted() {
  },
  computed: {
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
