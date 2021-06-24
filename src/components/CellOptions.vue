<template>
  <div>
    <p>Options</p>
    <b-form-radio-group
      v-model="selected"
      :options="options"
      class="mb-3"
      stacked
      v-on:change="$emit('change-option', selected)"
    ></b-form-radio-group>
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
      options: [0,1,2,3,4,5,6,7,8,9],
      selected: 0
    }
  },
  mounted() {
  },
  computed: {},
  methods: {
  }
}
</script>

<style scoped>
</style>
