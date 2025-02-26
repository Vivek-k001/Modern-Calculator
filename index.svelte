<script lang="ts">
    let display = "0";
    let firstValue: number | null = null;
    let operator: string | null = null;
  
    function appendNumber(num: string) {
      if (display === "0") {
        display = num;
      } else {
        display += num;
      }
    }
  
    function setOperator(op: string) {
      if (firstValue === null) {
        firstValue = parseFloat(display);  // Store the first number
      } else if (operator !== null) {
        calculate();  // Perform calculation if an operator is already set
        firstValue = parseFloat(display);  // Store the new number after calculation
      }
      operator = op;
      display = "0";  // Reset the display to 0 for the next number
    }
  
    function calculate() {
      if (firstValue !== null && operator !== null) {
        const secondValue = parseFloat(display);
        switch (operator) {
          case "+":
            display = (firstValue + secondValue).toString();
            break;
          case "-":
            display = (firstValue - secondValue).toString();
            break;
          case "*":
            display = (firstValue * secondValue).toString();
            break;
          case "/":
            display = (firstValue / secondValue).toString();
            break;
          default:
            break;
        }
        firstValue = null;
        operator = null;
      }
    }
  
    function clearDisplay() {
      display = "0";
      firstValue = null;
      operator = null;
    }
  </script>
  
  <main>
    <h1 id="title_name">CALCULATOR</h1>
  
    <div class="calculator">
      <div class="display">
        <p>{display}</p>
      </div>
      <div class="buttons">
        <button on:click={() => appendNumber("1")}>1</button>
        <button on:click={() => appendNumber("2")}>2</button>
        <button on:click={() => appendNumber("3")}>3</button>
        <button on:click={() => setOperator("+")}>+</button>
  
        <button on:click={() => appendNumber("4")}>4</button>
        <button on:click={() => appendNumber("5")}>5</button>
        <button on:click={() => appendNumber("6")}>6</button>
        <button on:click={() => setOperator("-")}>-</button>
  
        <button on:click={() => appendNumber("7")}>7</button>
        <button on:click={() => appendNumber("8")}>8</button>
        <button on:click={() => appendNumber("9")}>9</button>
        <button on:click={() => setOperator("*")}></button>
  
        <button on:click={() => appendNumber("0")}>0</button>
        <button on:click={clearDisplay}>C</button>
        <button on:click={calculate}>=</button>
        <button on:click={() => setOperator("/")}>/</button>
      </div>
    </div>
  </main>
  
  <style>
    main {
      text-align: center;
      font-family: Arial, sans-serif;
    }
    #title_name{
      color: #e0e1e1;
    }
  
    .calculator {
      display: inline-block;
      background-color: #f0f0f0;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 0px 4px 10px rgba(0.1, 0.1, 0.1, 0.1);
    }
  
    .display p {
      font-size: 2em;
      margin-bottom: 10px;
      min-width: 200px;
      height: 50px;
      line-height: 50px;
      background-color: #fff;
      border-radius: 5px;
      text-align: right;
      padding-right: 10px;
    }
  
    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 10px;
    }
  
    button {
      font-size: 1.5em;
      padding: 20px;
      background-color: #f9f9f9;
      border: 1px solid #ddd;
      border-radius: 5px;
      cursor: pointer;
      transition: all 0.5s ease; /* Smooth transition for all properties */
    }
  
    button:hover {
      background-color: #e0e0e0;
      transform: scale(1.1); /* Slightly enlarge the button on hover */
    }
  
    button:active {
      background-color: #c8c8c8;
      transform: scale(1); /* Ensure the button returns to normal size after being clicked */
    }
  </style>