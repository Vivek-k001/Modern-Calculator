<script>
  let displayValue = '0';
  let firstOperand = null;
  let operator = null;
  let darkMode = false;
  
  function handleNumberClick(number) {
    if (displayValue === '0') {
      displayValue = number.toString();
    } else {
      displayValue += number.toString();
    }
  }
  
  function handleOperatorClick(op) {
    firstOperand = parseFloat(displayValue);
    operator = op;
    displayValue = '0';
  }
  
  function handleEqualsClick() {
    const secondOperand = parseFloat(displayValue);
    let result = null;
  
    switch (operator) {
      case '+':
        result = firstOperand + secondOperand;
        break;
      case '-':
        result = firstOperand - secondOperand;
        break;
      case '*':
        result = firstOperand * secondOperand;
        break;
      case '/':
        result = firstOperand / secondOperand;
        break;
      default:
        break;
    }
  
    displayValue = result.toString();
    firstOperand = null;
    operator = null;
  }
  
  function handleClearClick() {
    displayValue = '0';
    firstOperand = null;
    operator = null;
  }
  
  function toggleDarkMode() {
    darkMode = !darkMode;
  }
  </script>
  
  <div class="calculator" class:dark={darkMode}>
    <button class="dark-mode-toggle" on:click={toggleDarkMode}></button>
    <div class="display">{displayValue}</div>
    <div class="buttons">
      <button on:click={handleClearClick}>C</button>
      <button on:click={() => handleNumberClick(7)}>7</button>
      <button on:click={() => handleNumberClick(8)}>8</button>
      <button on:click={() => handleNumberClick(9)}>9</button>
      <button on:click={() => handleOperatorClick('/')}>/</button>
      <button on:click={() => handleNumberClick(4)}>4</button>
      <button on:click={() => handleNumberClick(5)}>5</button>
      <button on:click={() => handleNumberClick(6)}>6</button>
      <button on:click={() => handleOperatorClick('*')}>*</button>
      <button on:click={() => handleNumberClick(1)}>1</button>
      <button on:click={() => handleNumberClick(2)}>2</button>
      <button on:click={() => handleNumberClick(3)}>3</button>
      <button on:click={() => handleOperatorClick('-')}>-</button>
      <button on:click={() => handleNumberClick(0)}>0</button>
      <button on:click={() => handleOperatorClick('+')}>+</button>
      <button on:click={handleEqualsClick}>=</button>
    </div>
  </div>
  
  <style>
  .calculator {
    width: 220px;
    height: 320px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f0f0f0;
    border: 1px solid #ccc;
    border-radius: 10px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    position: relative;
    transition: background-color 0.5s ease-in-out, border 0.5s ease-in-out;
  }
  
  .calculator.dark {
    background-color: #333;
    border: 1px solid #666;
  }
  
  .display {
    width: 92%;
    height: 35px;
    margin-bottom: 20px;
    padding: 10px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 20px;
    font-size: 24px;
    text-align: right;
    position: relative;
    z-index: 1;
  }
  
  .calculator.dark .display {
    background-color: #444;
    border: 1px solid #666;
    color: #fff;
  }
  
  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 10px;
  }
  
  button {
    width: 100%;
    height: 50px;
    padding: 10px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 18px;
    cursor: pointer;
    transition: background-color 0.3s ease-in-out;
  }
  
  .calculator.dark button {
    background-color: #555;
    border: 1px solid #777;
    color: #fff;
  }
  
  button:hover {
    background-color: #f5f5f5;
  }
  
  .calculator.dark button:hover {
    background-color: #666;
  }
  
  .dark-mode-toggle {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: grey;
    border: none;
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
    z-index: 2;
    transition: background-color 0.3s ease-in-out;
  }
  
  .calculator.dark .dark-mode-toggle {
    background-color: #777;
  }
  </style>
  