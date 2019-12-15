<template>
  <div id="app">
    <div class="quiz">
      <button class='quiz__start' @click='start' v-show='!gameNow && !finish'>Начать
        </button>
        <div class="quiz__content" v-show='gameNow && !finish'>
          <h2>Выберите правильный ответ</h2>
          <div class="quiz__question">
            {{ questions[questionIndex].text }}
          </div>
          <div class="quiz__answers" v-for='(answer, index) of questions[questionIndex].answers' :key='answer.index'  @click='chooseAnswer($event, index)'>
            {{ answer.text }}
          </div>
          <span>{{ questionIndex+1 + ' из ' + questions.length}}</span>
        </div>
        <div class="quiz__result" v-show='finish'>
          <h2>Результаты</h2>
          <div class="quiz__time">Вы прошли тест за {{totalTime}} сек.</div>
          <div class="quiz__total">Правильных ответов: {{score}}</div>
          <div class="quiz__passed">{{ score === 0 ? 'Вы не прошли тест :(' : score === 1 ? 'Неплохо, но могло быть лучше.' : 'Отличный результат!'}}</div>
          <button @click='restart' class="quiz__restart">Начать заново</button>
        </div>
    </div>
  </div>
</template>

<style lang="scss">

</style>
<script>
export default {
  data () {
    return {
      gameNow: false,
      questions: [
        {
          text: 'Куда держал путь Венедикт Ерофеев?',
          answers: [
            { text: 'Дагестан - Сочи' },
            { text: 'Питер - Калининград' },
            { text: 'Москва - Петушки', isCorrect: true },
            { text: 'Хабаровск - Архангельск' }
          ]
        },
        {
          text: 'Какую песню не написал Егор Летов?',
          answers: [
            { text: 'Винтовка - это праздник' },
            { text: 'Синеглазые дельфины', isCorrect: true },
            { text: 'Беспонтовый пирожок' },
            { text: 'Все идет по плану' }
          ]
        }
      ],
      questionIndex: 0,
      finish: false,
      startTime: 0,
      finishTime: 0,
      totalTime: 0,
      score: 0
    }
  },
  methods: {
    start () {
      this.gameNow = true
      this.startTime = Date.now()
    },
    restart () {
      this.gameNow = true
      this.finish = false
      this.questionIndex = 0
      this.startTime = Date.now()
      this.score = 0
    },
    chooseAnswer (e, index) {
      if ((this.questions[this.questionIndex].answers)[index].isCorrect === true) {
        this.score += 1
      }
      if (this.questionIndex < this.questions.length - 1) {
        this.questionIndex++
      } else {
        this.finish = true
        this.finishTime = Date.now()
        this.totalTime = (new Date(this.finishTime - this.startTime)).getSeconds()
      }
    }
  }
}
</script>
<style lang='scss'>
  * {
    margin: 0;
    padding: 0;
    font-family: Roboto, sans-serif;
  }
  html, body, #app {
    width: 100%;
    height: 100%;
  }
  .quiz {
    background: lightcyan;
    height: 100%;
    width: 100%;
    position: relative;
    &__start {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 200px;
      height: 45px;
      font-size: 20px;
      border-radius: 5px;
      cursor: pointer;
      background: cadetblue;
      color: #fff;
      &:hover {
        background: darkturquoise;
      }
    }
    &__content, .quiz__result {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
    }
    &__question {
      margin: 40px 0 20px;
      font-size: 18px;
      font-weight: bold;
    }
    &__answers, .quiz__restart {
      height: 40px;
      width: 100%;
      border: 1px solid transparent;
      background: cadetblue;
      color: #fff;
      border-radius: 10px;
      align-items: center;
      display: flex;
      justify-content: center;
      margin-bottom: 10px;
      cursor: pointer;
      &:hover {
        background: darkturquoise;
      }
    }
    &__result {
      & > div {
        margin-bottom: 10px
      }
      & > h2 {
        margin-bottom: 20px
      }
      & > button {
        font-size: 20px
      }
    }
  }
</style>
