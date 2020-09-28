<template>
  <div id="app">
    <Header
      :numCorrect="numCorrect"
      :numTotal="numTotal"
      @select-difficulty="getDifficulty"
    />
    <b-container class="bv-example-row d-flex justify-content-center">
      <b-row>
        <b-col cols="12">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :nextQuestion="nextQuestion"
            :increment="increment"
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
      difficulty: "",
    };
  },
  methods: {
    nextQuestion() {
      if (this.numTotal >= 10) {
        alert("Your final is score: " + this.numCorrect);
        this.getDifficulty(this.difficulty);
      } else {
        this.index++;
      }
    },
    increment(isCorrect) {
      this.numTotal++;

      if (isCorrect) {
        this.numCorrect++;
      }
    },
    getDifficulty(params) {
      this.difficulty = params;
      this.numCorrect = 0;
      this.numTotal = 0;
      this.index = 0;
      this.fetchQuestions(this.difficulty);
    },
    fetchQuestions(difficulty) {
      fetch(
        `https://opentdb.com/api.php?amount=10&category=11&difficulty=${difficulty}&type=multiple`,
        {
          method: "get",
        }
      )
        .then((response) => {
          return response.json();
        })
        .then((jsonData) => {
          this.questions = jsonData.results;
        });
    },
  },
  mounted: function () {
    this.fetchQuestions();
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
</style>
