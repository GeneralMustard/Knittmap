<template>
  <div>
    <b-img
      @click="modalShow = !modalShow"
      v-bind="colorPrevProps(initColor)"
      rounded
    ></b-img>

    <b-modal
      title="Pick a color"
      size="sm"
      v-model="modalShow"
      @show="resetModal"
      @ok="updateSelectedColor"
    > 
      <div class="d-flex justify-content-center">
        <chrome-picker 
          @input="updateModalColor" 
          v-model="modalColor" 
        />
      </div>
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
      modalColor: this.initColor,
      modalShow: false
    }
  },
  mounted() {},
  computed: {},
  methods: {
    colorPrevProps(color) {
      return { blank: true, blankColor: color, class: 'color-prev' }
    },
    updateModalColor(value) {
      this.modalColor = value.hex;
    },
    resetModal() {
      this.modalColor = this.initColor;
    },
    updateSelectedColor() {
      this.$emit('update-color-option', this.id, this.modalColor);
    }
  }
}
</script>

<style scoped>
  .color-prev {
    width: 40px;
    height: 40px;
    margin-left: 8px;
    outline-color: rgb(153, 153, 153);
    outline-style: outset;
  }
</style>
