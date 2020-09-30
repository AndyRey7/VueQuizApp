<template>
  <div class="questionbox-container">
    <b-jumbotron>
      <template slot="lead"> {{currentQuestion.question}} </template>

      <hr class="my-4" />



    <b-list-group>
        <b-list-group-item
        v-for="(answer, index) in answers" :key="index"
        @click.prevent="selectedAnswer(index)"
        :class="answerClass(index)"
        >
            {{answer}}
        </b-list-group-item>
    </b-list-group>

      <b-button
      variant="primary"
      @click="submitAns"
      :disabled="selectedIndex === null"
      >
        Submit
    </b-button>
      <b-button  @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAns: []
        }
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                {
                    this.selectedIndex = null
                    this.shuffleAns()
                }
            }
        }

    },
    methods: {
        selectedAnswer(index) {
            this.selectedIndex = index;

        },
        submitAns() {
            let isCorrect = false;
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
                this.increment(isCorrect)
            }
        },
        shuffleAns() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
             this.shuffledAns = _.shuffle(answers)
             this.correctIndex = this.shuffledAns.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index) {
            let answerClass = "";
            if (this.selectedIndex === index) {
                answerClass = "selected"
            } else if (this.correctIndex === index) {
                answerClass = "correctAns"
            } else if (this.correctIndex !== index && this.selectedIndex === index) {
                answerClass = "incorrectAns"
            }
            return answerClass
        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    }
}

</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }
    .list-group-item:hover {
        background-color: #EEE;
        cursor: pointer;
    }
    .btn {
        margin-left: 5px;
    }
    .correctAns {
        background-color: #45EE45;
    }
    .selected {
        background-color:#7676cc;
    }
    .incorrectAns {
        background-color: #aa4445;
    }
</style>
