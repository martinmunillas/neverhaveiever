<script>
  import statementsEs from "./statements/statements-es.json";
  import statementsEn from "./statements/statements-en.json";
  import i18n from "./i18n.json";

  let lang = "en";
  let history = [];
  let idx = 0;

  function getRandomStatement() {
    const randomIndex = Math.floor(Math.random() * statements.length);
    randomStatement = statements[randomIndex];
    history.unshift(randomStatement);
    history = history;
  }

  function getPreviousStatement() {
    idx = idx + 1;
    randomStatement = history[idx];
  }

  function getNextStatement() {
    idx = idx - 1;
    randomStatement = history[idx];
  }

  let randomStatement = "";
  let canPrevious = false;
  let canNext = false;
  let statements = statementsEn;
  $: {
    randomStatement = history[idx];
    canPrevious = idx < history.length - 1;
    canNext = idx > 0;
    statements = lang === "en" ? statementsEn : statementsEs;
  }
</script>

<main>
  <h1>{i18n.title[lang]}</h1>

  {#if history.length}
    <p>{randomStatement}</p>
    <div>
      <button on:click={getPreviousStatement} disabled={!canPrevious}
        >{i18n.previous[lang]}</button
      >
      <button on:click={getRandomStatement} class="random"
        >{i18n.random[lang]}</button
      >
      <button on:click={getNextStatement} disabled={!canNext}
        >{i18n.next[lang]}</button
      >
    </div>
  {:else}
    <select bind:value={lang}>
      <option value="en">English 🇺🇸</option>
      <option value="es">Español 🇪🇸</option>
    </select>
    <button on:click={getRandomStatement}>{i18n.start[lang]}</button>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 2rem;
    background: rgba(255, 255, 255, 0.1);
    border-radius: 10px;
    width: 80%;
    max-width: 600px;
    backdrop-filter: blur(10px);
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px;
  }

  h1 {
    color: #333333;
    text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.5);
  }

  p {
    color: #333333;
    font-weight: bold;
    text-shadow: 1px 1px 2px rgba(255, 255, 255, 0.5);
  }
  div {
    display: flex;
    justify-content: center;
  }

  button {
    background-color: rgba(255, 255, 255, 0.8);
    color: #333333;
    font-size: 1rem;
    padding: 0.5rem 1rem;
    margin: 0 0.5rem;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px;
    transition: background-color 0.2s ease-in-out, transform 0.2s ease-in-out;
  }

  button:hover {
    background-color: rgba(255, 255, 255, 1);
  }

  button:disabled {
    background-color: rgba(255, 255, 255, 0.6);
    color: #999999;
    cursor: not-allowed;
    box-shadow: none;
  }

  button.random {
    background-color: #ff3e00;
    color: white;
    font-weight: bold;
    transform: scale(1.1);
  }

  button.random:hover {
    background-color: #cc3200;
  }

  select {
    background-color: rgba(255, 255, 255, 0.8);
    color: #333333;
    font-size: 1rem;
    padding: 0.5rem;
    border: none;
    border-radius: 5px;
    box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px;
    backdrop-filter: blur(10px);
    appearance: none; /* Remove the default select styling */
  }

  select:focus {
    outline: none;
  }
</style>
