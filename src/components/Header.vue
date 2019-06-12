<template>
  <div>
    <b-nav tabs>
    <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
    <b-nav-item disabled>Counter: {{ numberCorrect }}/{{ numberOfQuestions }}</b-nav-item>
  </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    numberCorrect: Number,
    numberOfQuestions: Number,
    numberAnswered: Number
  }
}
</script>