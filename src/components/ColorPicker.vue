<template>
  <div>
    <b-img class="ml-2"
      @click="modalShow = !modalShow"
      v-bind="colorPrevProps(newColor)"
      rounded
    ></b-img>

    <b-modal
      v-model="modalShow"
      @show="resetModal"
      @ok="updateSelectedColor"
    > 
      <chrome-picker 
        class="color-picker" 
        @input="updateModalColor" 
        v-model="modalColor" 
        />
    </b-modal>
  </div>
</template>

<script>
import { Chrome } from 'vue-color'

export default {
  name: 'ColorPicker',
  props: {
    id: Number,
    initColor: String
  },
  components: {
    'chrome-picker' : Chrome
  },
  data() {
    return {
      newColor: this.initColor,
      modalColor: this.initColor,
      modalShow: false
    }
  },
  mounted() {},
  computed: {},
  methods: {
    colorPrevProps(color) {
      return { blank: true, blankColor: color, width: 40, height: 40, class: 'color-prev' }
    },
    updateModalColor(value) {
      this.modalColor = value.hex;
    },
    resetModal() {
      this.modalColor = this.newColor;
    },
    updateSelectedColor() {
      this.newColor = this.modalColor;
      this.$emit('update-color-option', this.id, this.newColor);
    }
  }
}
</script>

<style scoped>
  .color-picker {
    margin: 15px;
  }
</style>
