<template>
  <div>
    <Cell :id=5 />
    <Cell :id=3 />
    <Cell :id=7 />
    <Cell :id=4 />
    <Cell :id=5 />
    <Cell :id=1 />
    <Cell :id=0 />
    <Cell :id=2 />
    <Cell :id=6 />
    <Cell :id=9 />
  </div>
</template>

<script>
import Cell from '@/components/Cell.vue'

export default {
  name: 'Row',
  props: {
  },
  components: {
    Cell
  }
}
</script>

<style scoped>
</style>
