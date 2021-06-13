<template>
    <b-button squared size="sm" variant="outline-secondary">{{ this.id }}</b-button>
</template>

<script>
export default {
  name: 'Cell',
  props: {
    id: Number
  }
}
</script>

<style scoped>
</style>
