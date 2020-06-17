<template>
  <div id="app">
    <Header
    :numCorrectAnswers="numCorrectAnswers"
    :numTotal="numTotal"/>
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <question-box v-if="questions.length"
          :currentQuestion="questions[index]"
          :next="next"
          :increments="increments"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  import Header from "@/components/Header";
  import QuestionBox from "@/components/QuestionBox";

export default {
  name: 'App',
  components: {
      Header,
      QuestionBox
  },
  data() {
      return {
          questions: [],
          index: 0,
          numCorrectAnswers: 0,
          numTotal: 0
      }
  },
  mounted: function () {
      fetch('https://opentdb.com/api.php?amount=10&category=18&difficulty=medium&type=multiple', {
          method: 'get'
      }).then((response) => {
          return response.json();
      }).then((jsonData)=> {
          this.questions = jsonData.results;
      })
  },
  methods: {
      next() {
          this.index++
      },
      increments(isCorrect) {
          if (isCorrect) {
              this.numCorrectAnswers++
          }
          this.numTotal++
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
