<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Simple Calculator</title>
  <style>
    body {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: #222;
      margin: 0;
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    .calculator {
      background: #333;
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 8px 15px rgba(0,0,0,0.5);
      width: 320px;
    }
    #display {
      width: 100%;
      height: 60px;
      background: #000;
      color: #0ff;
      font-size: 2.5em;
      text-align: right;
      padding: 10px 15px;
      border-radius: 10px;
      box-sizing: border-box;
      margin-bottom: 20px;
      user-select: none;
      overflow-x: auto;
    }
    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }
    button {
      padding: 20px;
      font-size: 1.3em;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background: #444;
      color: #eee;
      transition: background-color 0.3s;
      user-select: none;
    }
    button:hover {
      background: #555;
    }
    button.operator {
      background: #0ff;
      color: #000;
    }
    button.operator:hover {
      background: #00cccc;
    }
    button.equal {
      background: #28a745;
      color: white;
      grid-column: span 2;
    }
    button.equal:hover {
      background: #218838;
    }
    button.clear {
      background: #dc3545;
      color: white;
    }
    button.clear:hover {
      background: #c82333;
    }
  </style>
</head>
<body>

  <div class="calculator">
    <div id="display">0</div>
    <div class="buttons">
      <button class="clear" id="clear">C</button>
      <button id="backspace">⌫</button>
      <button class="operator" data-op="/">÷</button>
      <button class="operator" data-op="*">×</button>

      <button data-num="7">7</button>
      <button data-num="8">8</button>
      <button data-num="9">9</button>
      <button class="operator" data-op="-">−</button>

      <button data-num="4">4</button>
      <button data-num="5">5</button>
      <button data-num="6">6</button>
      <button class="operator" data-op="+">+</button>

      <button data-num="1">1</button>
      <button data-num="2">2</button>
      <button data-num="3">3</button>
      <button class="equal" id="equals">=</button>

      <button data-num="0" style="grid-column: span 2;">0</button>
      <button data-num=".">.</button>
    </div>
  </div>

  <script>
    const display = document.getElementById('display');
    const buttons = document.querySelectorAll('button');
    let currentInput = '0';

    function updateDisplay() {
      display.textContent = currentInput;
    }

    function appendInput(char) {
      if (currentInput === '0' && char !== '.') {
        currentInput = char;
      } else {
        currentInput += char;
      }
    }

    function isOperator(char) {
      return ['+', '-', '*', '/'].includes(char);
    }

    buttons.forEach(button => {
      button.addEventListener('click', () => {
        if (button.id === 'clear') {
          currentInput = '0';
          updateDisplay();
        } else if (button.id === 'backspace') {
          if (currentInput.length > 1) {
            currentInput = currentInput.slice(0, -1);
          } else {
            currentInput = '0';
          }
          updateDisplay();
        } else if (button.id === 'equals') {
          try {
            // Evaluate expression safely
            let expression = currentInput.replace(/×/g, '*').replace(/÷/g, '/');
            let result = eval(expression);
            currentInput = String(result);
          } catch {
            currentInput = 'Error';
          }
          updateDisplay();
        } else if (button.dataset.num) {
          if (currentInput === 'Error') currentInput = '';
          if (button.dataset.num === '.' && currentInput.includes('.')) return;
          appendInput(button.dataset.num);
          updateDisplay();
        } else if (button.dataset.op) {
          if (currentInput === 'Error') currentInput = '0';
          const lastChar = currentInput.slice(-1);
          if (isOperator(lastChar)) {
            // Replace operator
            currentInput = currentInput.slice(0, -1) + button.dataset.op;
          } else {
            currentInput += button.dataset.op;
          }
          updateDisplay();
        }
      });
    });

    updateDisplay();
  </script>
</body>
</html>
