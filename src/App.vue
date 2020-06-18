<template>
  <div id="app">
    <div v-bind:class="display">
      <div :class="classCurrent">{{ current }}</div>

      <div :class="classResult">{{ parseInt(result) }}</div>

      <div :class="classComment">
        <div :class="classCommentInner">
          <div :class="classTextarea">{{ currentMessage }}</div>

          <div :class="classTriangle"></div>
        </div>

        <div :class="classCharacter">
          <img src="./assets/character.png" />
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
          <button v-on:click="clearClick" :class="clear">{{ buttons.clear }}</button>
        </li>
        <li>
          <button v-on:click="delClick" :class="del">{{ buttons.del }}</button>
        </li>
        <li v-for="sign in buttons.signs" :key="sign.index">
          <button v-on:click="signClick(sign)" :class="mark">{{ sign }}</button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentMessage: "ようこそ！",
      messages: [
        "電卓だよ！",
        "計算しよ〜〜、そ〜しよ〜〜",
        "Vue.jsを使っているよ"
      ],
      clickMessage: [
        "いてっ!",
        "たたくなよ〜",
        "やめろって",
        "やめてってば〜",
        "やめてよ..."
      ],
      current: 0,
      result: 0,
      log: 0,
      buttons: {
        nums: [9, 8, 7, 6, 5, 4, 3, 2, 1, "", 0, ""],
        signs: ["+", "-", "*", "/", "="],
        del: "del",
        clear: "clear"
      },
      inputs: {
        values: [],
        commands: []
      },
      calc: {
        sum: (a, b) => a + b,
        sub: (a, b) => a - b,
        mul: (a, b) => a * b,
        div: (a, b) => a / b
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
      classTextarea: "textarea",
      classTriangle: "triangle",
      classCommentInner: "comment-inner",
      classCharacter: "character"
    };
  },

  name: "App",

  methods: {
    keydownHandler: function(event) {
      console.log(event);
      switch (event.code.substring(0, 5)) {
        case "Digit": {
          const current = String(this.current) + event.key;
          this.current = parseInt(current);
          break;
        }
      }

      switch (event.key) {
        case "=":
        case "+":
        case "-":
        case "*":
        case "/":
          this.signClick(event.key);
          break;
        case "Enter":
          this.signClick("=");
          break;
        case "Backspace":
          this.delClick();
          break;
        case "Delete":
          this.clearClick();
          break;
      }
    },

    changeMessage: function() {
      let messages = this.messages.slice();

      // 同じコメントの連続表示を防止する
      if (this.messages.includes(this.currentMessage)) {
        const index = messages.indexOf(this.currentMessage);
        messages.splice(index, 1);
      }

      const random = getRandomInt(messages.length);
      this.currentMessage = messages[random];

      function getRandomInt(max) {
        return Math.floor(Math.random() * Math.floor(max));
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
    }
  },

  mounted: function() {
    window.addEventListener("keydown", this.keydownHandler);
    window.setInterval(this.changeMessage, 4000);
  }
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
img {
  max-width: 100%;
}

html {
  font-size: 62.5%;
}

@media (min-width: 768px) {
  html {
    font-size: 90%;
  }
}

body {
  background-color: black;
  font-size: 1.4em;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

.display {
  background: rgba(255, 255, 255, 0.9);
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
  border: none;
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

div.comment {
  display: flex;
  justify-content: flex-end;
}

.comment-inner {
  max-width: 16em;
}

div.character {
  width: 4em;
}

.comment .textarea {
  padding: 0.8em;
  padding-right: 1em;
  float: left;
  /*  */
  resize: none;
  border-radius: 0.5em;
  text-align: left;
  font-weight: bold;
  color: white;
  /*  */
  background-color: #b2ce54;
  /*  */
}

textarea:focus {
  outline: none;
}

.triangle {
  float: left;
  width: 0.4em;
  margin-top: 1em;
  border-top: 0.3em solid transparent;
  border-bottom: 0.3em solid transparent;
  border-left: 0.6em solid #b2ce54;
}
</style>
