<template>
  <div id="app">
    <input id="result" v-model.number="result" />
    <p>result is: {{ parseInt(result) }}</p>
    <ul>
      <li v-for="num in nums" :key="num.index">
        <button v-on:click="numClick(num)">{{ num }}</button>
      </li>
    </ul>
    <button v-on:click="calc">+1</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      result: 0,
      nums: [],
    };
  },

  name: "App",

  methods: {
    init: function() {
      for (let i = 0; i <= 9; i++) {
        this.nums.push(i);
      }
    },

    calc: function() {
      this.result++;
    },

    keydownHandler: function(event) {
      console.log(event.code);
      switch (event.code.substring(0, 5)) {
        case "Digit": {
          const result = String(this.result) + event.key;
          this.result = parseInt(result);
          break;
        }
      }
    },

    numClick: function(num) {
      console.log(num);
      const result = String(this.result) + String(num);
      this.result = parseInt(result);
    },
  },

  mounted: function() {
    window.addEventListener("keydown", this.keydownHandler);
    this.init();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
  list-style: none;
}

#result {
  text-align: right;
}
</style>
