<template>
  <div class="container">
    <ScoreBoard :winnerCount="this.winnerCount" :loserCount="this.loserCount" />

    <template v-if="this.question">
      <h1 v-html="this.question"></h1>

      <div id="answers" v-for="(answer, index) in this.answers" :key="index">
        <div
          class="answer-container"
          @click="selectAnswer(index)"
          :style="{ backgroundColor: clickedAnswer === index ? '#2900b0' : '#00b2cd' }"
        >
          <p v-html="answer"></p>
        </div>
      </div>

      <br />
      <button v-if="!this.answerSubmitted" @click="this.submitAnswer()" class="send">SEND</button>
      <section v-if="this.answerSubmitted" class="result">
        <h4
          v-if="this.chosenAnswer == this.correctAnswer"
          v-html="' &#9989; Congrats, ' + this.correctAnswer + 'is the correct answer!'"
        ></h4>
        <h4
          v-else
          v-html="
            '&#10060; I am sorry, you picked the wrong answer. The correct one is ' +
            this.correctAnswer
          "
        ></h4>
        <button @click="this.getNewQuestion()" class="send" type="button">NEXT QUESTION</button>
      </section>
    </template>
  </div>
</template>

<script>
import ScoreBoard from './components/ScoreBoard.vue'

export default {
  name: 'App',

  components: {
    ScoreBoard
  },

  data() {
    return {
      question: undefined,
      incorrectAnswers: [],
      correctAnswer: undefined,
      chosenAnswer: undefined,
      answerSubmitted: false,
      winnerCount: 0,
      loserCount: 0,
      clickedAnswer: null,
      isActive: false
    }
  },

  computed: {
    answers() {
      let answers = JSON.parse(JSON.stringify(this.incorrectAnswers))
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer)
      return answers
    }
  },

  methods: {
    selectAnswer(index) {
      if (this.clickedAnswer === index) {
        this.clickedAnswer = null

        // Reset the clicked answer
      } else {
        this.clickedAnswer = index
        this.chosenAnswer = this.answers[index]
        // Set the index of the clicked answer
      }
    },

    submitAnswer() {
      if (!this.chosenAnswer) {
        alert('Choose an option before sending the answer')
      } else {
        this.answerSubmitted = true
        if (this.chosenAnswer === this.correctAnswer) {
          this.winnerCount += 1
        } else {
          this.loserCount += 1
        }
      }

      if (this.winnerCount === 5) {
        alert('You are the winner')
        this.winnerCount = 0
        this.loserCount = 0
      } else if (this.loserCount === 5) {
        alert('The Computer is the winner')
        this.winnerCount = 0
        this.loserCount = 0
      }
    },

    getNewQuestion() {
      this.answerSubmitted = false
      this.chosenAnswer = undefined
      this.question = undefined
      this.clickedAnswer = null

      this.axios.get('https://opentdb.com/api.php?amount=1').then((response) => {
        this.question = response.data.results[0].question
        this.incorrectAnswers = response.data.results[0].incorrect_answers
        this.correctAnswer = response.data.results[0].correct_answer
      })
    }
  },

  created() {
    this.getNewQuestion()
  }
}
</script>

// https://opentdb.com/api.php?amount=1

<style lang="scss" scoped>
.container {
  width: 80%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: auto;
}

#answers {
  width: 80%;
  margin-top: 0.5rem;
  .answer-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 0.5rem 0;
    padding: 1rem;
    border-radius: 0.5rem;
    color: #f0f8ff;
    cursor: pointer;
    transition: ease 1s;

    &:hover {
      background-color: #2900b0;
    }
  }
}

h1 {
  text-align: center;
  margin-top: 40px;

  @media (max-width: 600px) {
    font-size: 1.5rem;
  }
}

h4 {
  text-align: center;
}

#answers {
  display: flex;
  align-items: center;
}

input[type='radio'] {
  margin: 12px 4px;
}

button.send {
  width: 80%;
  margin: 1rem auto;
  border: none;
  padding: 1rem;
  border-radius: 0.5rem;
  background-color: #2900b0;
  color: #f0f8ff;
  font-weight: bolder;
  cursor: pointer;
}

.result {
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
