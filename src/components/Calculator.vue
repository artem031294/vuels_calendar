<template>
  <div class="centered">
    <div class="calculator">
      <div class="display">{{current || '0'}}</div>
      <div @click="clear" class="btn">C</div>
      <div @click="sign" class="btn">+/-</div>
      <div @click="percent" class="btn">%</div>
      <div @click="divide" class="btn operator">/</div>
      <div @click="append(7)" class="btn">7</div>
      <div @click="append(8)" class="btn">8</div>
      <div @click="append(9)" class="btn">9</div>
      <div @click="multiply" class="btn operator">*</div>
      <div @click="append(4)" class="btn">4</div>
      <div @click="append(5)" class="btn">5</div>
      <div @click="append(6)" class="btn">6</div>
      <div @click="minus" class="btn operator">-</div>
      <div @click="append(1)" class="btn">1</div>
      <div @click="append(2)" class="btn">2</div>
      <div @click="append(3)" class="btn">3</div>
      <div @click="plus" class="btn operator">+</div>
      <div @click="append(0)" class="btn double">0</div>
      <div @click="dot()" class="btn">.</div>
      <div @click="equal" class="btn operator">=</div>
    </div>

    <div class="wrapper">

      <div>
      <fieldset>
        <legend>Бассейн</legend>
        <label for="pool25">25м</label>
        <input type="radio" id="pool25" name="pool" checked="true">
        <label for="pool50">50м</label>
        <input type="radio" id="pool50" name="pool">
      </fieldset>
      </div>
      <div>
      <fieldset>
        <legend>Пол</legend>
        <label for="male">Муж</label>
        <input type="radio" id="male" name="sex" checked="true">
        <label for="female">Жен</label>
        <input type="radio" id="female" name="sex">
      </fieldset>
      </div>
      <div>
        <fieldset>
        <legend>Стиль</legend>
        <select id="style">
          <option value="ml">Комплекс</option>
          <option value="fr">Вольный</option>
          <option value="bk">Спина</option>
          <option value="bf">Баттерфляй</option>
          <option value="br">Брасс</option>
        </select>
      </fieldset>
      </div>
      <div>
        <label for="min">Мин</label>
        <input type="number" value="0" id="min" min="0" max="59" step="1">
        <span class="validity"></span>
      </div>
      <div>
        <label for="sec">Сек</label>
        <input type="number" value="0" id="sec" min="0" max="59" step="1">
        <span class="validity"></span>
      </div>
      <div>
        <label for="mili">Сотые</label>
        <input type="number" value="00" id="mili" min="0" max="99" step="1">
        <span class="validity"></span>
      </div>
      <div class="result">
        <p>Ваши очки FINA: <span></span></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      current: '0',
      prev: null,
      operator: null,
      operatorClicked: false,
    }
  },
  methods: {
    clear() {
      this.current = '';
    },
    sign() {
      this.current = -1 * parseFloat(this.current);
    },
    percent() {
      this.current = `${parseFloat(this.current) / 100}`;
    },
    append(number) {
      if (this.operatorClicked) {
        this.current = '';
        this.operatorClicked = false;
      }

      this.current = `${this.current}${number}`;
    },
    dot() {
      if(this.current.indexOf('.') === -1){
        this.append('.');
      }
    },
    setPrev() {
      this.prev = this.current;
      this.operatorClicked = true;
    },
    multiply() {
      this.operator = (a,b) => a * b;
      this.setPrev();
    },
    divide() {
      this.operator = (a,b) => a / b;
      this.setPrev();
    },
    minus() {
      this.operator = (a,b) => a - b;
      this.setPrev();
    },
    plus() {
      this.operator = (a,b) => a + b;
      this.setPrev();
    },
    equal() {
      this.current = `${this.operator(
        parseFloat(this.current),
        parseFloat(this.prev)
      )}`;
      this.prev = null;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  * {
    font-family: OpenSans, sans-serif;
    margin: 0;
    padding: 0;
  }
  .centered {
    display: grid;
    margin: 0 auto;
  }
  .calculator {
    margin: 0 auto;
    width: 500px;
    font-size: 40px;
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
  }
  .wrapper {
    margin: 0 auto;
    width: 500px;
    font-size: 20px;
    display: inline-grid;
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: minmax(50px, auto);
  }
  .wrapper>div {
    text-align: center;
  }
  .result {
    grid-column: 1/4;
  }
  .calculator>div {
    padding: 10px;
    text-align: center;
  }
  .display {
    grid-column: 1/5;
    background-color: #333;
    color: white;
  }
  .double {
    grid-column: 1/3;
  }
  .btn {
    background-color: #f2f2f2;
    border: 1px solid #333;
  }
  .operator {
    background-color: orange;
    color: white;
  }
  input[type="number"]:invalid + span:after {
    content: '✖';
    color: #f00;
    padding-left: 5px;
  }
  input[type="number"]:valid + span:after {
    content: '✓';
    color: #26b72b;
    padding-left: 5px;
  }
  input[type="radio"] {
    margin-right: 5px;
  }
  label {
    font-size: .8rem;
    padding-right: 0.3rem;
  }
  .clear {
    clear: both;
  }
  fieldset {
    height: 50px;
    vertical-align: middle;
    line-height: 1.4rem;
    margin-top: 5px;
    padding: 10px;
    border: none;
  }
</style>
