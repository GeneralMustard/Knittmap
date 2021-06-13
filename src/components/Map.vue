<template>
  <div>
    <p id="name">{{ this.text }}</p>
    <div class="grid-container"
      v-for="cell in cells" v-bind:key="cell._id">
      <button class="grid-item" v-on:click="myFunction()">{{ cell }}</button>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Map',
  props: {
  },
  components: {
  },

  data() {
    return {
      number: 1,
      text: "hello saga",
      cells: [1,2,3,4,5,6,7,8,9],
      col: 3
    }
  },
  methods: {
    myFunction() {
       this.number = 2;
    },
    myFunction2() {
      this.text = "hello simon"
    }
  }
}
</script>

<style scoped>
.grid-container {
  display: inline-grid;
  grid-template-columns: auto auto auto;
  background-color: #2196F3;
  padding: 10px;
}

.grid-item {
  background-color: rgba(255, 255, 255, 0.8);
  border: 1px solid rgba(0, 0, 0, 0.8);
  padding: 20px;
  font-size: 30px;
  text-align: center;
}

.grid-item:hover {background-color: #3e8e41}

.button {
  padding: 15px 25px;
  font-size: 24px;
  text-align: center;
  cursor: pointer;
  outline: none;
  color: #fff;
  background-color: #04AA6D;
  border: none;
  border-radius: 15px;
  box-shadow: 0 9px #999;
}

.button:hover {background-color: #3e8e41}

.button:active {
  background-color: #3e8e41;
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
</style>
