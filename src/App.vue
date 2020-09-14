<template>
  <div id="app">
    <h1>Q<img alt="Vue logo" src="./assets/logo.png" height="100" />iz</h1>
    <Header :numCorrect="numCorrect" :numTotal="numTotal" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="8" offset="2">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :currentQIndex="index"
            :next="next"
            :increment="increment"
            :reset="reset"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
    reset() {
      this.index = 0;
      this.numCorrect = 0;
      this.numTotal = 0;
    },
  },
  mounted: function() {
    fetch("https://opentdb.com/api.php?amount=10&category=27&type=multiple", {
      method: "get",
    })
      .then((res) => {
        return res.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Cabin:wght@400;700&display=swap");
#app {
  font-family: Cabin, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 20px;
}
h1 {
  font-size: 3.2rem;
}
</style>
