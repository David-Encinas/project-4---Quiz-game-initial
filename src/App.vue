<template>
   <div>
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>
      <template v-if="this.question">
        
        <h1 v-html="this.question"></h1>
        
        <template v-for="answer in this.answers" :key="answer">
          
          <input
          :disabled="this.answerSubmitted"
          type="radio"
          name="options"
          :value="answer"
          :id="answer"
          v-model="this.chosenAnswer">
          
          <label :for="answer" v-html="answer"></label><br>
          
        </template>
        
        <button v-if="!this.answerSubmitted" @click="this.submitAnswer()" class="send" type="button">Send</button>


        <section class="result" v-if="this.answerSubmitted">
          <h4 v-if="this.chosenAnswer == this.correctAnswer" v-html="this.correctAnswer + ' is the correct answer'"></h4>
          <h4 v-if="this.chosenAnswer && this.chosenAnswer != this.correctAnswer" v-html="'Wrong. The correct answer is ' + this.correctAnswer"></h4>
          <button @click="this.newQuestion()" class="send" type="button">Next Question</button>
        </section>
        
      </template>
    </div>
</template>
  
<script>

import ScoreBoard from './components/ScoardBoard.vue';
export default {
  name: 'App',  
  components: {
    ScoreBoard
  },
  data() {
    return {
      question: undefined,
      incorrecteAnswers: undefined,
      correctAnswer: undefined,
      chosenAnswer: false, 
      answerSubmitted: false,    
      winCount: 0,
      loseCount: 0, 
    }
  },
  computed: {
    answers() {
      let answers = JSON.parse(JSON.stringify(this.incorrecteAnswers));
      answers.splice(Math.round(Math.random() * answers.length), 0, this.correctAnswer);
      return answers;
    }
  },  
  methods: {
    submitAnswer() { 
      if (!this.chosenAnswer) {
        console.log('pick an option')
      } else {
        this.answerSubmitted = 'true';
        if (this.chosenAnswer ==  this.correctAnswer) {
          this.winCount++;
        }
        if (this.chosenAnswer && this.chosenAnswer !=  this.correctAnswer) {
          this.loseCount++;
        }
      }
    },
    newQuestion() {
      this.question = false;
      this.incorrecteAnswers = undefined;
      this.correctAnswer = undefined;
      this.chosenAnswer = false;
      this.answerSubmitted = false;

      // https://opentdb.com/api.php?amount=1      
      this.axios.get('https://opentdb.com/api.php?amount=1').then((response) => {
        let data =  response.data.results[0];
        this.question = data.question;
        this.incorrecteAnswers = data.incorrect_answers;
        this.correctAnswer = data.correct_answer;
      })
    }
    
  },
  created() {
    this.newQuestion();
  }

}

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  max-width: 960px;
  margin: 60px auto;
  input[type='radio'] {
    margin: 12px 4px;
  }
  .send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
