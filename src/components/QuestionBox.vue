<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template slot="lead">
        {{ currentQuestion.question }}
      </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in shuffledAnswers" 
          :key="index"
          @click="selectAnswer(index)"
          :class="answerClass(index)"
        >
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" href="#" 
        @click="submitAnswer"
        :disabled="selectedIndex === null || answered"
      >
        Submit
      </b-button>
      <b-button variant="success" href="#" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import { shuffle } from 'lodash';

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data() {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      return answers;
    }
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffledAnswers = this.shuffleAnswers();
        this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        this.answered = false;
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    shuffleAnswers() {
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
      return shuffle(answers);
    },
    submitAnswer() {
      let isCorrect = this.selectedIndex === this.correctIndex;

      this.answered = true;

      this.increment(isCorrect);
    },
    answerClass(index) {
      let answerClass = '';

      if (!this.answered && this.selectedIndex === index) {
        answerClass = 'selected';
      } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = 'incorrect';
      } else if (this.answered && this.correctIndex === index) {
        answerClass = 'correct';
      }

      return answerClass;
    }
  }
}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #EEE;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}

.selected {
  background-color: lightblue;
}

.correct {
  background-color: lightgreen;
}

.incorrect {
  background-color: red;
}
</style>