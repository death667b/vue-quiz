<template>
  <div id="app">
    <Header 
      :numberCorrect="numberCorrect"
      :numberOfQuestions="numberOfQuestions"
      :numberAnswered="numberAnswered"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
            v-if="questions.length >= 1"
            :currentQuestion="questions[index]"
            :nextPage="nextPage"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  data() {
    return {
      questions: [],
      index: 0,
      numberCorrect: 0,
      numberAnswered: 0,
      numberOfQuestions: null
    }
  },
  methods: {
    nextPage() {
      const numQuestions = this.questions.length - 1;
      this.index = (this.index >= numQuestions) ? this.index : this.index + 1;
    },
    increment(isCorrect) {
      if (isCorrect) this.numberCorrect++;
      this.numberAnswered++;
    }
  },
  components: {
    Header,
    QuestionBox
  },
  mounted() {
    fetch('https://opentdb.com/api.php?amount=10&difficulty=easy&type=multiple', {
      method: 'get'
    })
      .then(res => res.json())
      .then(jsonData => {
        this.questions = jsonData.results;
        this.numberOfQuestions = jsonData.results.length;
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
