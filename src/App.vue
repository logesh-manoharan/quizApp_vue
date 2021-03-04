<template>
  <div>
    <NavigationComponent 
    :numCorrect="numCorrect"
    :numTotal="numTotal"/>
    <QuizboxComponent 
      v-if="questions.length"
      :currentQuestion="questions[index]"
      :next="next"
      :increment="increment"
    />
  </div>
</template>

<script>
import NavigationComponent from "./components/NavigationComponent";
import QuizboxComponent from './components/QuizboxComponent';
export default {
  name: 'App',
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  components: {
    NavigationComponent,
    QuizboxComponent
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&category=21&difficulty=easy&type=multiple', {
      method: 'get'
    })
    .then((response) => {
      return response.json();
    })
    .then((jsonData) => {
      this.questions = jsonData.results
    })
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
    }
  }
}
</script>

