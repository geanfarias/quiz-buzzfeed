<template>
  <div>
    <div class="questionBlock" v-for="(item, index) in valores" :key="index" data-teste="pergunta">
      <div class="card-image">
        <figure class="image is-4by3">
          <img
            src="https://i.pinimg.com/originals/c9/d3/bb/c9d3bb26e1f6cacf59adf3070ab28749.jpg"
            alt="Placeholder image"
          />
        </figure>
        <div class="img-title">
          <h3 class="title">{{item.question}}</h3>
        </div>
      </div>
      <nav class="panel">
        <label class="panel-block" v-for="(option, index) in item.options" :key="index">
          <input
            data-teste="opcao"
            type="radio"
            :value="[index]"
            :name="[item.questionNumber]"
            @click="verifyUserAnswer($event, index, item.questionNumber)"
          />
          {{option}}
        </label>
      </nav>
    </div>
    <div class="results">
      <h4>{{AllResult}}</h4>
      <button class="button is-primary" @click="cleanForm()">Refazer</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      correctAnswers: 0,
      optQuestion: 0,
      valores: [
        {
          question: "Qual a capital do Brasil?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctIndex: 2,
          questionNumber: 0
        },
        {
          question: "Qual a capital do Amazonas?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctIndex: 1,
          questionNumber: 1
        },
        {
          question: "Qual a capital da Bahia?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctIndex: 0,
          questionNumber: 2
        },
        {
          question: "Qual a capital de São Paulo?",
          options: ["Salvador", "Manaus", "Brasília", "São Paulo"],
          correctIndex: 3,
          questionNumber: 3
        }
      ]
    };
  },
  methods: {
    verifyUserAnswer: function(evento, userAnswer, question) {
      let inputsAnswer = document.querySelectorAll(`input[name="${question}"]`);
      let correctQuestion = this.$data.valores[question].correctIndex;
      if (userAnswer === correctQuestion) {
        inputsAnswer.forEach(function(elemento) {
          elemento.disabled = true;
        });
        evento.target.parentElement.classList.add("has-background-primary");
        this.$data.correctAnswers++;
        this.$data.optQuestion++;
        if(this.$data.optQuestion == this.$data.valores.length){
          document.getElementsByClassName("results")[0].classList.add('active')
        }
      } else {
        evento.target.parentElement.classList.add("has-background-danger");
        inputsAnswer.forEach(function(elemento) {
          elemento.disabled = true;
          if (elemento.value == correctQuestion) {
            elemento.parentElement.classList.add("has-background-primary");
          };
        });
        this.$data.optQuestion++;
        if(this.$data.optQuestion == this.$data.valores.length){
          document.getElementsByClassName("results")[0].classList.add('active')
        };
      }
    },
    cleanForm: function(){
      let questions = document.querySelectorAll(`input`);
      questions.forEach(function(elemento){
        elemento.disabled = false;
        elemento.parentElement.classList.remove("has-background-primary");
        elemento.parentElement.classList.remove("has-background-danger");
      });
    }
  },
  computed: {
    AllResult() {
      if(this.$data.correctAnswers != 0){
        return "Não foi dessa vez. Tente novamente."
      }
      else{
        return `Você acertou ${this.$data.correctAnswers} de ${this.$data.valores.length}, Parabéns!`;
      }
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
  visibility: hidden;
  opacity: 0;
  transition: all 0.4s;

  &.active {
    visibility: visible;
    opacity: 1;
  }
}
</style>