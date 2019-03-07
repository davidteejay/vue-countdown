<template>
<div>
  <div class="countdown">
    <h1 v-if="!timeUp">{{ minute }}:{{ second < 10 ? '0' + second : second }}</h1>
    <h1 v-else>Time's up</h1>
  </div>
  <!-- This is the questions div. Note that the v-for is used to load all the questions and the isCurrent method is used to display the current question -->
  <div class="quiz" :class="{show: isCurrent(i)}" v-for="(question, i) in questions" :key="i">
    <h3>{{ question.question }}</h3>
    <p>
      <!-- Note the isChecked method. This is used to check if the question has been answered and preselect the answered option -->
      <!-- The answer method is used to answer the question -->
      <!-- The name method is used to give a different name to each question, so the preselection is not overridden -->
      <input :checked="isChecked('a')" @change="answer('a')" id="optionA" type="radio" :name="name(i + 1)">
      <label for="optionA">{{ question.optionA }}</label>
    </p>
    <p>
      <input :checked="isChecked('b')" @change="answer('b')" id="optionB" type="radio" :name="name(i + 1)">
      <label for="optionB">{{ question.optionB }}</label>
    </p>
    <p>
      <input :checked="isChecked('c')" @change="answer('c')" id="optionC" type="radio" :name="name(i + 1)">
      <label for="optionC">{{ question.optionC }}</label>
    </p>
    <p>
      <input :checked="isChecked('d')" @change="answer('d')" id="optionD" type="radio" :name="name(i + 1)">
      <label for="optionD">{{ question.optionD }}</label>
    </p>
  </div>
  <!-- The go method is used to go back or go forward -->
  <button @click="go(-1)">PREVIOUS</button>
  <button @click="go(1)">NEXT</button>
</div>
</template>

<script>
import questions from './test'

export default {
  name: 'HelloWorld',
  data(){
    return {
      minute: 20,
      second: 0,
      timeUp: false,
      questions: [],
      currentQuestion: 1,
      answers: []

    }
  },
  methods: {
    isCurrent: function(index){
      return this.timeUp === false && this.currentQuestion === index + 1
    },
    go: function(dir){
      let { currentQuestion, questions } = this
      if (dir === -1 && currentQuestion === 1) alert("This is the first question")
      else if (dir === 1 && currentQuestion === questions.length) alert("This is the last question")
      else this.currentQuestion = currentQuestion + dir
    },
    answer: function(answer){
      let { answers, currentQuestion } = this

      // This gets the index of the current question in the answer array and replaces the previous answer with the new one
      let search = answers.findIndex(answer => {
        return answer.question === currentQuestion
      })

      answers[search] = { question: currentQuestion, answer }

      this.answers = answers
    },
    name: function(option){
      // This returns a unique string for each question
      return `${option}option`
    },
    isChecked: function(option){
      // This returns true if the answer in the answer array is equal to the option
      return this.answers[this.currentQuestion - 1].answer === option
    }
  },
  mounted(){
    this.questions = questions

    // This is used to populate the answers array with empty strings. Just to make it easier
    for (let i = 0; i < questions.length; i++){
      this.answers.push({ question: i + 1, answer: '' })
    }

    let timer = setInterval(() => {
      let { minute, second } = this

      if (second === 0 && minute === 0){
        clearInterval(timer)
        this.timeUp = true
      } else {
        if (second === 0){
          second = 59
          minute--
        } else second--

        this.second = second
        this.minute = minute
      }
    }, 1000)
  }
}
</script>

<style scoped>
.countdown {
  display: flex;
  align-items: center;
  justify-content: center
}

h1 {
  width: 100%
}

.quiz {
  display: none
}

.quiz.show {
  display: block;
}
</style>
