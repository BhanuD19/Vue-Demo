<template>
  <div>
    <b-jumbotron>

      <template v-slot:lead>
        {{currentQuestion.question}}
      </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffledAnswers" :key="index"
        @click.prevent="selectAnswer(index)"
        :class="answerClass(index)">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary"
      @click="submitAnswer"
      :disabled="selectedIndex === null || isSubmitted"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
  import _ from 'lodash'

    export default {
        name: "QuestionBox",
        props: {
            currentQuestion: Object,
            next: Function,
            increments: Function
        },
        data: function () {
            return {
                selectedIndex: null,
                shuffledAnswers: [],
                correctIndex: null,
                isSubmitted: false
            }
        },
        computed: {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers];
                answers.push(this.currentQuestion.correct_answer);
                return answers;
            }
        },

        watch: {
            currentQuestion: {
                immediate: true,
                handler() {
                    this.selectedIndex = null
                    this.isSubmitted = false
                    this.shuffleAnswers()
                }
            }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index
            },
            shuffleAnswers(){
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer() {
                let isCorrect = false

                if(this.selectedIndex === this.correctIndex) {
                    isCorrect = true
                }
                this.isSubmitted = true
                this.increments(isCorrect)
            },
            answerClass(index) {
                let answerClass= ''
                if(!this.isSubmitted && this.selectedIndex === index) {
                    answerClass= 'selected'
                } else if (this.isSubmitted && this.correctIndex === index) {
                    answerClass = 'correct'
                }else if( this.isSubmitted && this.selectedIndex === index && this.correctIndex !== index ) {
                    answerClass = 'incorrect'
                }
                return answerClass
            }
        }
    }
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: antiquewhite;
  cursor: pointer;
}
.btn {
  margin: 0 5px;
}
.selected {
    background-color: cornflowerblue;
  }
.correct {
  background-color: aquamarine;
}
.incorrect {
  background-color: crimson;
}
</style>
