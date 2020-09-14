<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>Vue.js QViz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: ["numCorrect", "numTotal"],
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.nav {
  margin-bottom: 30px;
  font-size: 1.1rem;
}
</style>
