<template>
  <div>
    <b-sidebar
      id="sb-info"
      ref="sbinforef"
      right bg-variant="dark"
      text-variant="light"
      aria-labelledby="sidebar-no-header-title"
      backdrop
    >
      <p class="px-3 py-2 text-left">
        With Knittmap you can design and preview patterns.
        Start by creating a new map with the option 'New' at the top right corner
          (you can also load an existing map and save your current map here).
        Use the options to the right to manage the colors you want to use.
        Activate an option by pressing the number.
        You can now mark all places in the pattern for this color.
        Add or delete rows or columns by pressing the number for this row or column.
        Use 'Preview' to check the repition of your pattern.
        <br><br>
        Example:
      </p>

      <b-img class="pr-2 pl-2 pb-2" fluid
        src="../assets/example/example_1.png"/>
      <b-button variant="light" v-on:click="loadExample()"
      >Load example</b-button>
    </b-sidebar>
  </div>
</template>

<script>
export default {
  name: 'Info',
  methods: {
    loadExample() {
      this.$refs.sbinforef.hide();
      this.$emit('load-example');
    }
  }
}
</script>