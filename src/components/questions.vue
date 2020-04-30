<template>
  <div>
    <div class="questionBlock" v-for="(item, index) in questions" :key="index">
      <div class="card-image">
        <figure class="image is-4by3">
          <img
            src="https://i.pinimg.com/originals/c9/d3/bb/c9d3bb26e1f6cacf59adf3070ab28749.jpg"
            alt="Placeholder image"
          />
        </figure>
        <div class="img-title">
          <h3 class="title">{{ item.question }}</h3>
        </div>
      </div>
      <nav class="panel" :data-resposta="[dataResposta]" data-teste="pergunta">
        <label
          @click="onClickOption(item, indexAlternativa)"
          class="panel-block"
          v-for="(option, indexAlternativa) in item.options"
          :key="indexAlternativa"
          :class="{'has-background-primary' : item.isFinishedQuestion && indexAlternativa == item.correctAnswer, 
          'has-background-danger' : item.isFinishedQuestion && !item.isUserCorrect && indexAlternativa == item.userIndexAnswer}"
        >
          {{ option.isFinishedQuestion }}
          <input
            data-teste="opcao"
            type="radio"
            :value="[indexAlternativa]"
            :name="[index]"
            @click="verifyUserAnswer($event, indexAlternativa, index)"
          />
          {{ option }}
        </label>
      </nav>
    </div>
    <div
      class="results"
      :data-resultado="[dataResultado]"
      :class="{ active: totalAnswered == questions.length }"
    >
      <div>
        <span v-if="correctUserAnswers != 0">
          Você acertou {{ correctUserAnswers }} de {{ questions.length }}
          <span
            v-if="correctUserAnswers == questions.length"
          >Excelente!</span>
        </span>
        <span v-if="correctUserAnswers === 0">Infelizmente você não acertou nada. Tente novamente.</span>
      </div>
      <button class="button is-primary" @click="cleanForm()" data-test="refazer">Refazer</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      correctUserAnswers: 0,
      totalAnswered: 0,
      actual: "",
      questions: [
        {
          id: 0,
          question: "Qual a capital do Brasil?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctAnswer: 2,
          userIndexAnswer: "",
          isUserCorrect: false,
          isFinishedQuestion: false
        },
        {
          id: 1,
          question: "Qual a capital do Amazonas?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctAnswer: 1,
          userIndexAnswer: "",
          isUserCorrect: false,
          isFinishedQuestion: false
        },
        {
          id: 2,
          question: "Qual a capital da Bahia?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctAnswer: 0,
          userIndexAnswer: "",
          isUserCorrect: false,
          isFinishedQuestion: false
        },
        {
          id: 3,
          question: "Qual a capital de São Paulo?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctAnswer: 3,
          userIndexAnswer: "",
          isUserCorrect: false,
          isFinishedQuestion: false
        }
      ]
    };
  },
  methods: {
    verifyUserAnswer: function(evento, userAnswer, questionNumber) {
      let inputsAnswer = document.querySelectorAll(
        `input[name="${questionNumber}"]`
      );
      this.actual = this.questions[questionNumber];
      let correctQuestion = this.questions[questionNumber].correctAnswer;
      if (userAnswer === correctQuestion) {
        this.actual.isUserCorrect = true;
        inputsAnswer.forEach(function(elemento) {
          elemento.disabled = true;
        });
        // evento.target.parentElement.classList.add("has-background-primary");
        this.correctUserAnswers++;
        this.totalAnswered++;
      } else {
        // evento.target.parentElement.classList.add("has-background-danger");
        inputsAnswer.forEach(function(elemento) {
          elemento.disabled = true;
          if (elemento.value == correctQuestion) {
            // elemento.parentElement.classList.add("has-background-primary");
          }
        });
        this.totalAnswered++;
      }
    },
    onClickOption(item, userAnswer) {
      item.isFinishedQuestion = true;
      item.userIndexAnswer = userAnswer;
    },
    cleanForm: function() {
      window.scroll({
        top: 0,
        left: 0,
        behavior: "smooth"
      });
      this.correctUserAnswers = 0;
      this.totalAnswered = 0;
      let questions = document.querySelectorAll(`input`);
      questions.forEach(elemento => (elemento.disabled = false));
      this.questions.forEach(
        elemento => (
          (elemento.isUserCorrect = false),
          (elemento.isFinishedQuestion = false)
        )
      );
    }
  },
  computed: {
    dataResposta() {
      return this.actual.isUserCorrect ? "correta" : "errada";
    },
    dataResultado() {
      return this.correctUserAnswers;
    }
  }
};
</script>

<style lang="scss" scoped>
.img-title {
  position: absolute;
  color: white;
  top: 0;
  left: 0;
  right: 0;
  height: 100%;
  align-items: center;
  justify-content: center;
  display: flex;

  .title {
    width: 100%;
    background: white;
  }
}
input[type="radio"] {
  visibility: hidden;
  position: absolute;
}
.questionBlock {
  padding: 20px 0px;
  &:first-child {
    padding-top: 0;
  }
}
.results {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  visibility: hidden;
  opacity: 0;
  transition: all 0.4s;

  &.active {
    visibility: visible;
    opacity: 1;
  }
}
</style>
