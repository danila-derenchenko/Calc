<template>
  <div>
    <div>
      <input
        @click="active_input(1)"
        type="number"
        placeholder="0"
        v-model.number="operand1"
      />
      <input
        @click="active_input(2)"
        placeholder="0"
        v-model.number="operand2"
        type="number"
      />
      = {{ result }}
    </div>
    <div class="error" v-show="error">
      {{ error }}
    </div>
    <!-- <div class="error" v-show="error">
      {{ error }}
    </div> -->
    <div class="strange-message">
      <template v-if="result < 0">Отрицательное число</template>
      <template v-else-if="result < 100">Число меньше 100</template>
      <template v-else>Число больше 100</template>
    </div>
    <div>
      <button @click="action('+')">+</button>
      <button @click="action('-')">-</button>
      <button v-on:click="action('*')">*</button>
      <button @click="action('/')">/</button>
      <button @click="action('a*a')">Возведение op1 в степень op2</button>
      <button @click="calculate()">=</button>
      <button @click="c()">C</button>

      <!-- <button @click="eventFv">Event</button> -->
    </div>
    <div>
      <label for="check-keyboard">Экранная клавиатура</label>
      <input type="checkbox" v-model="keyboard" name="check-keyboard" />
    </div>
    <div v-show="keyboard">
      <button @click="addNumber(N)" v-for="N in values" :key="N">
        {{ N }}
      </button>
      <button @click="backspace()">Backspace</button>
    </div>
    <div style="display: flex; justify-content: center">
      <label for="op1">Операнд 1</label>
      <input type="radio" name="op1" value="1" v-model="active_i" />
      <label for="op2">Операнд 2</label>
      <input type="radio" name="op2" value="2" v-model="active_i" />
    </div>
  </div>
</template>

<script>
// Николай, я немного переделал калькулятор, теперь поля инпута реагируют на клики по ним, и делают их активными для ввода с экранной клавиатуры, так же активный инпут можно изменить с помощью радиокнопок
// Также, чтобы посчитать что - то, нужно кликнуть на первый инпут, ввести число(можно как руками, так и с экранной клавиатуры), потом выбрать действие, ввести второе число, и после нажатия на = вы увидите результат
// Для обнуления калькулятора добавил кнопку C
// В общем постарался сделать его более удобным
export default {
  name: "Calc",
  data: () => ({
    operand1: "",
    operand2: "",
    result: 0,
    active_i: 1,
    action_i: "",
    keyboard: false,
    error: false,
  }),
  methods: {
    backspace() {
      if (this.active_i == 1) {
        this.operand1 = Number(String(this.operand1).slice(0, -1));
      } else {
        this.operand2 = Number(String(this.operand2).slice(0, -1));
      }
    },
    c() {
      this.error = false;
      this.result = 0;
      this.action_i = "";
      this.operand1 = "";
      this.operand2 = "";
      this.active_i = 1;
    },
    action(act) {
      this.action_i = act;
    },
    active_input(active) {
      this.active_i = active;
      // alert(`Клик. Активное поле: ${this.active_i}`);
    },
    calculate() {
      this.error = false;
      this.operand1 = Number(this.operand1);
      this.operand2 = Number(this.operand2);
      switch (this.action_i) {
        case "+":
          this.result = this.operand1 + this.operand2;
          break;
        case "-":
          this.result = this.operand1 - this.operand2;
          break;
        case "*":
          this.result = this.operand1 * this.operand2;
          break;
        case "/":
          if (this.operand2 == 0) {
            this.error = "На ноль делить нельзя!";
            this.result = "Ошибка";
          } else {
            this.result = Math.round(this.operand1 / this.operand2);
          }
          break;
        case "a*a":
          this.result = Math.pow(this.operand1, this.operand2);
          break;
      }
    },

    addNumber(N) {
      if (this.active_i == 1) {
        this.operand1 = this.operand1 + `${N}`;
      } else {
        this.operand2 = this.operand2 + `${N}`;
      }
    },

    /* eventFv() {
      console.log(arguments);
    }, */
  },
  computed: {
    values: () => {
      let numbers = [];
      for (let i = 0; i <= 9; i++) {
        numbers.push(i);
      }
      return numbers;
    },
  },
};
</script>

<style scoped>
.error {
  color: red;
}
</style>