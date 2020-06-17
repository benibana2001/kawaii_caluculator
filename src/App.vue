<template>
  <div id="app">
    <div v-bind:class="[display]">
      <div>{{ current }}</div>
      <p>resutl is: {{ parseInt(result) }}</p>
    </div>

    <div>
      <ul>
        <li v-for="num in buttons.nums" :key="num.index">
          <button v-on:click="numClick(num)">{{ num }}</button>
        </li>
      </ul>

      <ul>
        <li v-for="sign in buttons.signs" :key="sign.index">
          <button v-on:click="signClick(sign)">{{ sign }}</button>
        </li>
        <li>
          <button v-on:click="delClick">{{ buttons.del }}</button>
        </li>
        <li>
          <button v-on:click="clearClick">{{ buttons.clear }}</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: 0,
      result: 0,
      log: 0,
      buttons: {
        nums: [],
        signs: ["+", "-", "*", "/", "="],
        del: "del",
        clear: "clear",
      },
      inputs: {
        values: [],
        commands: [],
      },
      calc: {
        sum: (a, b) => a + b,
        sub: (a, b) => a - b,
        mul: (a, b) => a * b,
        div: (a, b) => a / b,
      },
      display: "display",
    };
  },

  name: "App",

  methods: {
    init: function() {
      for (let i = 0; i <= 9; i++) {
        this.buttons.nums.push(i);
      }
    },

    keydownHandler: function(event) {
      console.log(event.code);
      switch (event.code.substring(0, 5)) {
        case "Digit": {
          const current = String(this.current) + event.key;
          this.current = parseInt(current);
          break;
        }
      }
    },

    getResult: function() {
      if (this.inputs.values.length > 1) {
        const func = this.inputs.commands[this.inputs.commands.length - 2];
        return func(
          this.result,
          this.inputs.values[this.inputs.values.length - 1]
        );
      } else {
        return this.inputs.values[0];
      }
    },

    numClick: function(num) {
      console.log(num);
      const current = String(this.current) + String(num);
      this.current = parseInt(current);
    },

    end: function() {
      this.current = this.result;
      this.result = 0;
      this.inputs.values = [];
      this.inputs.commands = [];
    },

    reset: function() {
      this.current = 0;
      this.result = 0;
      this.inputs.values = [];
      this.inputs.commands = [];
    },

    backspaceHandler: function() {
      if (this.current) {
        const str = String(this.current);
        const result = parseInt(str.substring(0, str.length - 1));
        if (result) {
          this.current = result;
        } else {
          this.current = 0;
        }
      }
    },

    delClick: function() {
      this.backspaceHandler();
    },

    clearClick: function() {
      this.reset();
    },

    signClick: function(sign) {
      let func = () => {};
      console.log(sign);

      switch (sign) {
        case "+":
          func = this.calc.sum;
          break;
        case "-":
          func = this.calc.sub;
          break;
        case "*":
          func = this.calc.mul;
          break;
        case "/":
          func = this.calc.div;
          break;
        case "=":
          break;
      }

      this.inputs.values.push(this.current);
      this.inputs.commands.push(func);

      this.result = this.getResult();

      if (sign === "=") {
        this.end();
      } else {
        this.current = 0;
      }
    },
  },

  mounted: function() {
    window.addEventListener("keydown", this.keydownHandler);
    this.init();
  },
};
</script>

<style>
html,
body,
h1,
h2,
h3,
h4,
p,
ul,
li,
a {
  margin: 0;
  padding: 0;
}
li {
  list-style: none;
}
a {
  text-decoration: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* margin-top: 60px; */
}

#current {
  text-align: right;
}

.display {
  background: rgba(200, 200, 200, 0.8);
  text-align: right;
  font-size: 4em;
}
</style>
