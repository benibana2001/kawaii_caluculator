<template>
  <div id="app">
    <div v-bind:class="display">
      <div :class="classCurrent">{{ current }}</div>

      <div :class="classResult">{{ parseInt(result) }}</div>

      <div :class="classComment">
        <textarea>メッセージ</textarea>

        <div>
          <!-- <img src="./assets/character.jpg" /> -->
          character
        </div>
      </div>
    </div>

    <div v-bind:class="[container]">
      <ul :class="left">
        <li v-for="num in buttons.nums" :key="num.index">
          <button v-on:click="numClick(num)" :class="num">{{ num }}</button>
        </li>
      </ul>

      <ul :class="right">
        <li>
          <button v-on:click="clearClick" :class="clear">
            {{ buttons.clear }}
          </button>
        </li>
        <li>
          <button v-on:click="delClick" :class="del">
            {{ buttons.del }}
          </button>
        </li>
        <li v-for="sign in buttons.signs" :key="sign.index">
          <button v-on:click="signClick(sign)" :class="mark">
            {{ sign }}
          </button>
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
        nums: [9, 8, 7, 6, 5, 4, 3, 2, 1, "", 0, ""],
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
      container: "container",
      left: "left",
      right: "right",
      mark: "mark",
      del: "del",
      clear: "clear",
      classCurrent: "current",
      classResult: "result",
      classComment: "comment",
    };
  },

  name: "App",

  methods: {
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
a,
p,
textarea {
  margin: 0;
  padding: 0;
}
li {
  list-style: none;
}
a {
  text-decoration: none;
}

html {
  font-size: 62.5%;
}

body {
  background-color: black;
  font-size: 1.6em;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.display {
  background: rgba(200, 200, 200, 0.8);
  text-align: right;
  height: 30vh;
}

.current {
  font-size: 4em;
}

.result {
  font-size: 2em;
  color: #555;
}

.container {
  display: flex;
  width: 100%;
  height: 70vh;
}

.contailer li {
}

.left {
  display: flex;
  width: 75%;
  flex-flow: row-reverse wrap;
}

.left li {
  width: calc(100% / 3);
}

.left li button {
  width: 100%;
  height: 100%;
}

.right {
  display: flex;
  width: 25%;
  flex-flow: row wrap;
}

.right li {
  width: 100%;
}

.right li button {
  width: 100%;
  height: 100%;
}

button {
  font-size: 1.4em;
  background-color: #444;
  color: white;
  font-size: 1.6em;
  font-weight: lighter;
}
button:focus {
  outline: none;
}

button.num {
}

button.mark {
  background-color: #555;
}

button.clear {
  background-color: #999;
}

button.del {
  background-color: #666;
}

.comment {
  display: flex;
}

.comment textarea {
  width: 70%;
  margin: 1em;
  padding: 0.6em;
  /*  */
  resize: none;
  border: 0px;
  border-radius: 0.5em;
  font-weight: bold;
  color: yellowgreen;
  /*  */
}

textarea:focus {
  outline: none;
}

.comment > div {
  width: 30%;
}
</style>
