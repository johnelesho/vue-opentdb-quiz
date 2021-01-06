<template>
<div>
  <Header :nCorrect="nCorrect" :nTotal="nTotal"/>
  <b-container class="bv-example-row">
  <b-row>
    <b-col sm="6" offset="3">
      <QuestionBox v-if="questions.length" :increment="increment" :next="next" :currentQuestion="questions[index]"/></b-col>

  </b-row>
</b-container>

</div>

</template>

<script>
import Header from './components/Header.vue'
import  QuestionBox from './components/QuestionBox'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions:[],
      index: 0,
      nCorrect:0,
      nTotal:0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.nCorrect++
      }
      this.nTotal++
    }
  },
  mounted() {
fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple',{
  method: 'get'
})
.then(response=>{
 return response.json()
})
.then(data=>{
  this.questions = data.results
})
  },
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
