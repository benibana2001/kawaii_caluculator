<template>
  <div>
    <h2>Sample Text: {{ msg }}</h2>
    <h2>Sample Num: {{ parseInt(num) + 1 }}</h2>
    <a v-bind:href="url">link</a>
  </div>
</template>

<script>
export default {
  name: "Sample",
  data() {
    return {
      url: "http://google.com",
    };
  },
  props: {
    msg: String,
    num: String,
  },
};
</script>

<style scoped>
h2 {
  color: darkslategray;
}
</style>
