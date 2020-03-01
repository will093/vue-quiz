<template>
  <div id="app">
    <Header
      :numTotal="numTotal"
      :numCorrect="numCorrect"
    />
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import QuestionBox from './components/QuestionBox.vue';

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      index: 0,
      questions: [],
      numCorrect: 0,
      numTotal: 0,
    };
  },
  methods: {
    next() {
      this.index += 1; 
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect += 1;
      }
      this.numTotal += 1;
    }
  },
  async mounted() {
    try {
      const response = await fetch('https://opentdb.com/api.php?amount=10');
      const responseBody = await response.json();
      this.questions = responseBody.results;
    } catch (error) {
      console.error('Error', error);
    }
  }
}
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
