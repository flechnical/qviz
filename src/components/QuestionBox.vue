<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>
        {{ currentQuestion.question }}
      </template>
      <hr class="my-4" />
      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
          :disabled="submitted"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>
      <b-button
        v-if="currentQIndex !== 9 && selectedIndex === null"
        variant="secondary"
        disabled
      >
        Select an answer above
      </b-button>
      <b-button
        :disabled="currentQIndex === 9 && submitted"
        v-if="(!submitted && selectedIndex !== null) || currentQIndex === 9"
        @click="checkAnswer"
        variant="primary"
      >
        Submit
      </b-button>
      <b-button
        :disabled="currentQIndex === 9"
        v-if="submitted"
        @click="next"
        variant="outline-primary"
      >
        Next
      </b-button>
      <b-button
        v-if="currentQIndex === 9 && submitted"
        @click="reset"
        variant="dark"
      >
        Retry
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    currentQuestion: Object,
    currentQIndex: Number,
    next: Function,
    increment: Function,
    reset: Function,
  },
  data() {
    return {
      correctIndex: null,
      selectedIndex: null,
      shuffledAnswers: [],
      submitted: false,
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion() {
      this.correctIndex = null;
      this.selectedIndex = null;
      this.shuffleAnswers();
      this.submitted = false;
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(index);
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    checkAnswer() {
      let isCorrect = false;
      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.increment(isCorrect);
      this.submitted = true;
    },
    answerClass(index) {
      let answerClass = "";
      if (!this.submitted && index === this.selectedIndex) {
        answerClass = "selected";
      } else if (this.submitted && index === this.correctIndex) {
        answerClass = "correct";
      } else if (this.submitted && index === this.selectedIndex) {
        answerClass = "incorrect";
      }
      return answerClass;
    },
  },
  mounted() {
    this.shuffleAnswers();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.list-group {
  margin-bottom: 20px;
  font-size: 1.1rem;
}
.list-group-item:hover {
  background-color: rgba(255, 255, 255, 0.8);
  cursor: pointer;
}
.list-group-item:active {
  background-color: rgba(255, 255, 255, 0.4);
}
.selected,
.selected:hover {
  background-color: rgba(255, 255, 255, 0.4);
}
.correct {
  background-color: rgb(65 184 131);
  opacity: 1;
  color: white;
  text-shadow: 1px 1px #32966a, -1px -1px #32966a, -1px 1px #32966a,
    1px -1px #32966a;
}
.incorrect {
  background-color: rgb(53 73 94);
  opacity: 1;
  color: white;
}
.btn {
  margin: 0 5px;
}
.btn.disabled,
.btn:disabled {
  opacity: 0.2;
}
.btn-secondary.disabled,
.btn-secondary:disabled {
  opacity: 0.65;
}
</style>
