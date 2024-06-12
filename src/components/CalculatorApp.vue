html
Copy code
<template>
  <div class="calculator">
    <div class="display">{{ display }}</div>
    <div class="buttons">
      <button v-for="button in buttons" :key="button" @click="handleClick(button)">
        {{ button }}
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      display: '',
      buttons: [
        '7', '8', '9', '/', 'sin', 'cos', 'tan',
        '4', '5', '6', '*', 'log', 'ln', '^',
        '1', '2', '3', '-', '(', ')', 'sqrt',
        '0', '.', '=', '+', 'C', '←'
      ]
    };
  },
  methods: {
    handleClick(button) {
      if (button === 'C') {
        this.display = '';
      } else if (button === '=') {
        this.calculate();
      } else if (button === '←') {
        this.display = this.display.slice(0, -1);
      } else if (['sin', 'cos', 'tan', 'log', 'ln', 'sqrt'].includes(button)) {
        this.display += button + '(';
      } else if (button === '^') {
        this.display += '**';
      } else {
        this.display += button;
      }
    },
    calculate() {
      try {
        this.display = this.evaluateExpression(this.display);
      } catch (e) {
        this.display = 'Error';
      }
    },
    evaluateExpression(expression) {
      // Replace common scientific functions with Math methods
      expression = expression.replace(/sin\(/g, 'Math.sin(');
      expression = expression.replace(/cos\(/g, 'Math.cos(');
      expression = expression.replace(/tan\(/g, 'Math.tan(');
      expression = expression.replace(/log\(/g, 'Math.log10(');
      expression = expression.replace(/ln\(/g, 'Math.log(');
      expression = expression.replace(/sqrt\(/g, 'Math.sqrt(');
      
      return eval(expression);
    }
  }
};
</script>

<style scoped>
.calculator {
  max-width: 400px;
  margin: 0 auto;
  text-align: center;
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 8px;
}

.display {
  font-size: 2em;
  margin-bottom: 20px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
  word-wrap: break-word;
  text-align: right;
}

.buttons {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 10px;
}

button {
  font-size: 1.5em;
  padding: 20px;
  border: none;
  border-radius: 8px;
  background-color: #eee;
  cursor: pointer;
}

button:hover {
  background-color: #ddd;
}
</style>