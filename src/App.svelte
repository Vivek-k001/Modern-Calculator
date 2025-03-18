<script>
  let displayValue = '0';
  let liveAnswer = '0';
  let mode = 'light'; // 'light', 'dark', 'rgb'
  let showHistory = false;
  let history = [];
  let clickSound = new Audio('click.mp3'); // Ensure this file exists in your project

  function updateLiveAnswer() {
    try {
      let result = eval(displayValue.replace('%', '/100'));
      liveAnswer = isNaN(result) ? '' : result.toString();
    } catch {
      liveAnswer = '';
    }
  }

  function handleNumberClick(number) {
    clickSound.play();
    if (displayValue === '0') {
      displayValue = number.toString();
    } else {
      displayValue += number.toString();
    }
    updateLiveAnswer();
  }

  function handleOperatorClick(op) {
    clickSound.play();
    displayValue += ` ${op} `;
    updateLiveAnswer();
  }

  function handleParenthesesClick() {
    clickSound.play();
    const openCount = (displayValue.match(/\(/g) || []).length;
    const closeCount = (displayValue.match(/\)/g) || []).length;
    displayValue += openCount <= closeCount ? '(' : ')';
    updateLiveAnswer();
  }

  function handleEqualsClick() {
    clickSound.play();
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
    clickSound.play();
    displayValue = '0';
    liveAnswer = '0';
    history = [];
  }

  function toggleMode() {
    if (mode === 'light') {
      mode = 'dark';
    } else if (mode === 'dark') {
      mode = 'rgb';
    } else {
      mode = 'light';
    }
  }

  function toggleHistory() {
    showHistory = !showHistory;
  }
</script>

<div class="calculator" class:dark={mode === 'dark' || mode === 'rgb'} class:rgb={mode === 'rgb'}>
  <button class="mode-toggle" on:click={toggleMode}></button>

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
  @keyframes rgb-glow {
    0% { box-shadow: 0 0 10px red; }
    25% { box-shadow: 0 0 10px blue; }
    50% { box-shadow: 0 0 10px green; }
    75% { box-shadow: 0 0 10px yellow; }
    100% { box-shadow: 0 0 10px red; }
  }
 

  .calculator {
    font-family: sans-serif;
    width: 220px;
    height: 388px;
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

  .calculator.rgb {
    background-color: #222;
    animation: rgb-glow 2s infinite alternate;
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
    transition: box-shadow 0.5s ease-in-out;
  }

  .calculator.dark .display {
    background-color: #444;
    border: 1px solid #666;
    color: #fff;
  }

  .calculator.rgb .display {
    animation: rgb-glow 1.5s infinite alternate;
  }

  .buttons {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 10px;
    margin-top: 10px;
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
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.15);
    transition: transform 0.1s ease-in-out, background-color 0.3s ease-in-out;
  }

  .calculator.dark button {
    background-color: #555;
    border: 1px solid #777;
    color: #fff;
  }

  .calculator.rgb button {
    animation: rgb-glow 1.5s infinite alternate;
  }

  button:active {
    transform: scale(0.95);
  }

  .mode-toggle {
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
    transition: background-color 0.3s ease-in-out;
  }

  .calculator.dark .mode-toggle {
    background-color: #777;
  }
</style>
