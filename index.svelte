<script>
  let displayValue = '0';
  let liveAnswer = '0'; // Live answer value
  let darkMode = false;
  let showHistory = false;
  let history = [];

  function updateLiveAnswer() {
    try {
      let result = eval(displayValue.replace('%', '/100'));
      liveAnswer = isNaN(result) ? '' : result.toString();
    } catch {
      liveAnswer = '';
    }
  }

  function handleNumberClick(number) {
    if (displayValue === '0') {
      displayValue = number.toString();
    } else {
      displayValue += number.toString();
    }
    updateLiveAnswer();
  }

  function handleOperatorClick(op) {
    displayValue += ` ${op} `;
    updateLiveAnswer();
  }

  function handleParenthesesClick() {
    const openCount = (displayValue.match(/\(/g) || []).length;
    const closeCount = (displayValue.match(/\)/g) || []).length;
    displayValue += openCount <= closeCount ? '(' : ')';
    updateLiveAnswer();
  }

  function handleEqualsClick() {
    try {
      let result = eval(displayValue.replace('%', '/100'));
      history = [`${displayValue} = ${result}`, ...history].slice(0, 5);
      displayValue = result.toString();
      liveAnswer = result.toString();
    } catch {
      displayValue = 'Error';
      liveAnswer = '';
    }
  }

  function handleClearClick() {
    displayValue = '0';
    liveAnswer = '0';
    history = [];
  }

  function toggleDarkMode() {
    darkMode = !darkMode;
  }

  function toggleHistory() {
    showHistory = !showHistory;
  }
</script>

<div class="calculator" class:dark={darkMode}>
  <button class="dark-mode-toggle" on:click={toggleDarkMode}></button>

  <div class="display">
    <div class="live-answer">{liveAnswer}</div>
    <div class="main-display">{displayValue}</div>
  </div>

  <div class="buttons">
    {#if !showHistory}
      <button on:click={handleClearClick}>C</button>
      <button on:click={handleParenthesesClick}>()</button>
      <button on:click={() => handleOperatorClick('%')}>%</button>
      <button on:click={() => handleOperatorClick('/')}>/</button>

      <button on:click={() => handleNumberClick(7)}>7</button>
      <button on:click={() => handleNumberClick(8)}>8</button>
      <button on:click={() => handleNumberClick(9)}>9</button>
      <button on:click={() => handleOperatorClick('*')}>*</button>

      <button on:click={() => handleNumberClick(4)}>4</button>
      <button on:click={() => handleNumberClick(5)}>5</button>
      <button on:click={() => handleNumberClick(6)}>6</button>
      <button on:click={() => handleOperatorClick('-')}>-</button>

      <button on:click={() => handleNumberClick(1)}>1</button>
      <button on:click={() => handleNumberClick(2)}>2</button>
      <button on:click={() => handleNumberClick(3)}>3</button>
      <button on:click={() => handleOperatorClick('+')}>+</button>

      <button on:click={toggleHistory}>H</button>
      <button on:click={() => handleNumberClick(0)}>0</button>
      <button on:click={() => handleOperatorClick('.')}>.</button>
      <button on:click={handleEqualsClick}>=</button>
    {/if}

    {#if showHistory}
      <div class="history-box">
        <h3>History</h3>
        {#each history as item}
          <p>{item}</p>
        {/each}
        <button on:click={toggleHistory}>Close</button>
      </div>
    {/if}
  </div>
</div>

<style>
  .calculator {
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", Helvetica, Arial, sans-serif;
    width: 220px;
    height: 400px;
    margin: 20px auto;
    padding: 20px;
    background-color: #f0f0f0;
    border: 1px solid #ccc;
    border-radius: 10px;
    box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
    position: relative;
    transition: background-color 0.5s ease-in-out, border 0.5s ease-in-out;
  }

  .calculator.dark {
    background-color: #333;
    border: 1px solid #666;
  }

  .display {
    width: 92%;
    height: 50px;
    padding: 10px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 20px;
    font-size: 24px;
    position: relative;
    z-index: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-end;
  }

  .calculator.dark .display {
    background-color: #444;
    border: 1px solid #666;
    color: #fff;
  }

  .live-answer {
    font-size: 14px;
    color: #888;
    position: absolute;
    top: 5px;
    right: 12px;
  }

  .calculator.dark .live-answer {
    color: #bbb;
  }

  .main-display {
    font-size: 22px;
    font-weight: bold;
    margin-top: 15px;
  }

  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 10px;
    margin-top: 10px;
  }

  button {
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", Helvetica, Arial, sans-serif;
    width: 100%;
    height: 50px;
    padding: 10px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 18px;
    cursor: pointer;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
    transition: background-color 0.3s ease-in-out, box-shadow 0.3s ease-in-out;
  }

  .calculator.dark button {
    background-color: #555;
    border: 1px solid #777;
    color: #fff;
  }

  button:hover {
    background-color: #f5f5f5;
    box-shadow: 0 3px 7px rgba(0, 0, 0, 0.2);
  }

  .calculator.dark button:hover {
    background-color: #666;
    box-shadow: 0 3px 7px rgba(255, 255, 255, 0.2);
  }

  .dark-mode-toggle {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background-color: grey;
    border: none;
    position: absolute;
    top: 5px;
    right: 10px;
    cursor: pointer;
    z-index: 2;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    transition: background-color 0.3s ease-in-out;
  }

  .calculator.dark .dark-mode-toggle {
    background-color: #777;
  }
</style>
