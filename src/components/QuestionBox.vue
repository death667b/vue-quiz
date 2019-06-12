<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        <span v-html="currentQuestion.question "></span>
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in answersArray" :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          <span v-html="answer"></span>
        </b-list-group-item>
      </b-list-group>

      <b-button 
        variant="primary"
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button @click="nextPage" variant="success">
        Next
      </b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    nextPage: Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex: null,
      answersArray: [],
      correctIndex: null,
      answered: false
    }
  },
  computed: {
    answers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers, 
        this.currentQuestion.correct_answer];
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      this.answersArray = _.shuffle([
        ...this.currentQuestion.incorrect_answers, 
        this.currentQuestion.correct_answer]);
      this.correctIndex = this.answersArray.indexOf(this.currentQuestion.correct_answer);
    },
    submitAnswer() {
      let isCorrect = false;
      this.answered = true;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }

      this.increment(isCorrect);
    },
    answerClass(index) {
      return [
        !this.answered && this.selectedIndex === index ? 'selected' : 
        this.answered && this.correctIndex === index ? 'correct' : 
        this.answered && this.correctIndex !== index && this.selectedIndex === index ? 'incorrect' : ''
      ]
    }
  }
}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
  background-color: rgb(196, 196, 255);
}
.selected:hover {
  background-color: rgb(159, 159, 253);
}
.correct {
  background-color: lightgreen;
}
.correct:hover {
  background-color: rgb(92, 226, 92);
}
.incorrect {
  background-color: rgb(255, 143, 143);
}
.incorrect:hover {
  background-color: rgb(253, 120, 120);
}
</style>