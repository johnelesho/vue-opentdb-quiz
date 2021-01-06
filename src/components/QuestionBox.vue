<template name="question-box-container">
  <div>
  <b-jumbotron >


    <template #lead>
    {{ currentQuestion.question}}
    </template>

    <hr class="my-4">

<b-list-group>
  <b-list-group-item
   @click.prevent="selectAnswer(index)"
   class="mb-3"
   :class="answerClass(index)"
   v-for="(answer, index) in shuffledAnswers"
   :key="index">
      {{answer}}
  </b-list-group-item>

</b-list-group>


    <b-button variant="primary" :disabled="selectedIndex ===null || answered" @click="submitAnswer">Submit</b-button>
    <b-button  class="ml-3"
    @click="next"
    variant="success">Next</b-button>
  </b-jumbotron>
</div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next:Function,
    increment: Function
  },
  data() {
    return {
      selectedIndex : null,
      correctIndex: null,
      shuffledAnswers:[],
      answered: false,

    }
  },
  watch: {
    currentQuestion:{
      immediate: true,
      handler(){
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }

    // (){
    //   this.selectedIndex = null
    //   this.shuffleAnswers()
    // }
  },
  methods: {
    selectAnswer(index){
      this.selectedIndex = index
    },
    shuffleAnswers(){
    let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },

    submitAnswer(){
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex){
        isCorrect = true
      }

      this.answered = true

      this.increment(isCorrect)
    },
    answerClass(index){
      let answerClass = ''

      if (!this.answered && this.selectedIndex=== index) {
        answerClass = 'selected'
      } else if(this.answered && this.correctIndex === index ) {
        answerClass = 'correct'

      }
    else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
     answerClass = 'incorrect'
    }

    return answerClass
    }
  },
  computed: {
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      return answers
  },
  },
}
</script>

<style scoped>
  .selected{
    background-color: lightblue;
  }

  .correct{
    background:lightgreen;
  }
  .incorrect{
    background: red;
  }
</style>