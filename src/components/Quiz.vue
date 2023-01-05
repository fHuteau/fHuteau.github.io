<template>
  <h1>Quiz</h1>
  <div>
    <select v-model="number">
      <option>5</option>
      <option selected>10</option>
      <option>25</option>
      <option>50</option>
    </select>

    <select v-model="category">
      <option value="any">Any Category</option>
      <option value="9">General Knowledge</option>
      <option value="10">Entertainment: Books</option>
      <option value="11">Entertainment: Film</option>
      <option value="12">Entertainment: Music</option>
      <option value="13">Entertainment: Musicals &amp; Theatres</option>
      <option value="14">Entertainment: Television</option>
      <option value="15">Entertainment: Video Games</option>
      <option value="16">Entertainment: Board Games</option>
      <option value="17">Science &amp; Nature</option>
      <option value="18">Science: Computers</option>
      <option value="19">Science: Mathematics</option>
      <option value="20">Mythology</option>
      <option value="21">Sports</option>
      <option value="22">Geography</option>
      <option value="23">History</option>
      <option value="24">Politics</option>
      <option value="25">Art</option>
      <option value="26">Celebrities</option>
      <option value="27">Animals</option>
      <option value="28">Vehicles</option>
      <option value="29">Entertainment: Comics</option>
      <option value="30">Science: Gadgets</option>
      <option value="31">Entertainment: Japanese Anime &amp; Manga</option>
      <option value="32">Entertainment: Cartoon &amp; Animations</option>
    </select>

    <select v-model="difficulty">
      <option value="any">Any Difficulty</option>
      <option value="easy">Easy</option>
      <option value="medium">Medium</option>
      <option value="hard">Hard</option>
    </select>

    <select v-model="type">
      <option value="any">Any Type</option>
      <option value="multiple">Multiple Choice</option>
      <option value="boolean">True / False</option>
    </select>

    <button v-on:click="getQuiz">New Quiz</button>
  </div>
  <div>
    <form @submit="onSubmit">
      <div v-for="(question, indexQ) in questions">
        <div class="form-label" :id="'question' + indexQ">
          {{ indexQ + 1 + '. ' + question.question }}
        </div>
        <div>
          <div class="form-check" v-for="(response, indexR) in question.reponses">
            <input class="form-check-input" type="radio" :id="'reponse' + indexQ + indexR" :value="response"
                   v-model="selectedAnswers[indexQ]">
            <label class="form-check-label" :for="'reponse' + indexQ + indexR">{{ response }}</label>
          </div>
        </div>
        <br>
      </div>
      <input v-if="hasQuestion" type="submit" value="Valider"/>
      <h3 v-else>No questions found</h3>
    </form>
  </div>
</template>

<script>
import $ from 'jquery';
import he from 'he'
export default {
  name: "quiz",
  data() {
    this.getQuiz();
    return {
      number: 10,
      category: 'any',
      difficulty: 'any',
      type: 'any',
      questions: [],
      selectedAnswers: [],
      hasQuestion: true,
    }
  },

  methods: {
    newQuiz: async function () {
      let url = 'https://opentdb.com/api.php?amount=';

      if (this.number !== undefined) {
        url = url + this.number;
      } else {
        url = url + 10;
      }

      if (this.category !== 'any' && this.category !== undefined) {
        url = url + '&category=' + this.category;
      }

      if (this.difficulty !== 'any' && this.difficulty !== undefined) {
        url = url + '&difficulty=' + this.difficulty;
      }

      if (this.type !== 'any' && this.type !== undefined) {
        url = url + '&type=' + this.type;
      }

      return $.ajax({
        url: url,
        dataType: 'json',
        error: function (e) {
          console.log("error", e)
        }
      });
    },

    getQuiz: async function () {
      const result = await this.newQuiz();
      this.questions = result["results"].map(function (quiz) {
        Quiz["question"] = he.decode(Quiz["question"]);

        let response = Quiz["incorrect_answers"].map(function (answer) {
          return he.decode(answer);
        });
        response.push(he.decode(Quiz["correct_answer"]));

        Quiz["reponses"] = response.sort((a, b) => 0.5 - Math.random());

        return Quiz;
      });

      this.hasQuestion = this.questions.length > 0;
    },

    onSubmit: function (e) {
      e.preventDefault();
      for (let i = 0; i < this.number; i++) {
        const answer = he.decode(this.questions[i]["correct_answer"]) === this.selectedAnswers[i];

        if (answer) {
          document.getElementById("question" + i).style.background = "green"
        } else {
          document.getElementById("question" + i).style.background = "red"
        }

      }
    }
  }
}
</script>

<style scoped>

</style>