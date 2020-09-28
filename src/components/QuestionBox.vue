<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot:lead>
        <div v-html="currentQuestion.question"></div>
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers"
          :key="index"
          @click.prevent="selectAnswer(index)"
          :class="answerClass(index)"
        >
          <label v-html="answer"></label>
        </b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="nextQuestion" variant="success" :disabled="isSubmited == false">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash"; //Por convenio hay usarlo con _

export default {
  props: {
    currentQuestion: Object,
    nextQuestion: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
      isSubmited: false
    };
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.isSubmited = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;
      this.isSubmited = true;
      this.increment(isCorrect);
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
    answerClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex
      ) {
        answerClass = "incorrect";
      }

      return answerClass;
    },
  },
};
</script>

<!--scoped: Estos estilos afectaran solo al componente-->
<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: rgb(179, 174, 174);
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background: lightblue;
}
.correct {
  background: lightgreen;
}
.incorrect {
  background: lightcoral;
}
</style>