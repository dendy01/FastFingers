<template>
  <div class="container">
    <form>
      <input class="input" type="text" v-model="inputText" @input="checkText" @click="run" placeholder="Начать тест">
    </form>
    <div class="content">
      <h4 v-if="text.length <= 0">Сгенерируйте предложение</h4>
      <span v-for="(char, index) in text" :key="index" :style="{ color: char.color }">{{ char.value }}</span>
    </div>
    <div class="info">
      <ul class="infoUl">
        <li>Оставшееся время: <b>{{ time }}</b></li>
        <li>Верные нажатия: <b>{{ correctClick }}</b></li>
        <li>Неверные нажатия: <b>{{ incorrectClick }}</b></li>
        <li>Аккуратность: <b>{{ accuracy }}</b>%</li>
      </ul>
      <button class="btn" v-on:click="generateText">Сгенерировать предложение</button>
      <button class="btn" v-on:click="stop">Остановить</button>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        texts: [
          'Не следует, однако, забывать о том, что постоянный количественный рост и сфера нашей активности представляет собой интересный эксперимент проверки экономической целесообразности принимаемых решений!',
          'Робот теряет время просто так.',
          'Но я ожидаю, что новая неделя продолжит тренд повышенного спроса, — рассказал руководитель отдела контента каталога Onlíner Евгений Вербицкий.',
          'Моя девушка теряет время просто так.',
          'Вы только узнали, что на протяжении всех ваших путешествий вы забыли про закрытие дверей позади вас.',
          'Кроме того, вызвать пожар могут и школьники, работающие за детским лабораторным столом.'
        ],
        text: [],
        inputText: '',
        nextChar: 0,
        time: 60,
        timer: '',
        correctClick: 0,
        incorrectClick: 0,
        accuracy: 0
      }
    },

    methods: {
      generateText() {
        this.text = [];
        this.nextChar = 0;
        const newTexts = this.texts[Math.floor(Math.random() * this.texts.length)];

        for (let i = 0; i < newTexts.length; i += 1) {
          const newText = {
            value: '',
            color: '',
          };

          newText.value = newTexts[i].toLowerCase();
          newText.color = '';

          this.text.push(newText);
        }

        clearInterval(this.timer);

        this.correctClick = 0;
        this.incorrectClick = 0;
        this.inputText = '';
        this.time = 60;
        this.accuracy = 0;
      },

      checkText(event) {
        if (event.inputType === 'deleteContentBackward') {

          this.nextChar -= 1;
          this.text[this.nextChar].color = 'black';

        } else if (this.text.length > 0) {

          if (this.inputText[this.nextChar] === this.text[this.nextChar].value) {
            this.text[this.nextChar].color = 'green';
            this.nextChar += 1;
            this.correctClick += 1;
          } else {
            this.text[this.nextChar].color = 'red';
            this.nextChar += 1;
            this.incorrectClick += 1;
          }

        }
      },

      run() {
        if (this.inputText.length >= 0) {
          this.timer = setInterval(() => {
            this.time -= 1;

            if (this.time <= 0) {
              this.stop();
            }

          }, 1000);
        }
      },

      stop() {
        clearInterval(this.timer);
        this.accuracy = ((this.correctClick * 100) / (this.correctClick + this.incorrectClick)).toFixed(2);
      }
    }
  }
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  .container {
    width: 600px;
    height: 400px;
    border: 2px solid red;
    padding: 20px;
    margin: 10px auto;
  }

  .input {
    width: 100%;
    font-family: Arial Narrow, sans-serif;
    font-weight: bold;
    padding: 10px;
  }

  .content {
    height: 100px;
    padding: 20px;
    margin-top: 20px;
    border: 3px solid #00BFFF;
    font-family: Arial Narrow, sans-serif;
    font-weight: bold;
  }

  .info {
    margin-top: 50px;
    padding: 10px;
    display: flex;
    justify-content: space-between;
  }

  .infoUl {
    list-style-type: none;
  }

  .btn {
    height: 40px;
    background: none;
    padding: 10px;
    border: 3px solid #00BFFF;
    border-radius: 5px;
  }
</style>
