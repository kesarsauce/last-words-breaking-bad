<script>
  import Countdown from "./lib/Countdown.svelte";
  let death = { last_words: "Guess the person who said these last words" };
  let guess = "";
  let won = false;
  let lost = false;
  let out = [];

  async function fetchAllDeaths() {
    won = false;
    let response = await fetch("https://www.breakingbadapi.com/api/deaths");
    out = await response.json();
    out = out.filter(filterDeaths);
  }

  function getNewDeath() {
    guess = "";
    won = false;
    const random = Math.floor(Math.random() * out.length);
    death = out[random];
  }

  function filterDeaths(death) {
    let invalidLastWords = ["", "unknown", "unknwon"];
    if (invalidLastWords.includes(death.last_words.toLowerCase())) {
      return false;
    }
    if (death.death.toLowerCase() == "declan") {
      return false;
    }

    return true;
  }

  function validateGuess() {
    if (guess != undefined) {
      if (guess.toLowerCase() == death.death.toLowerCase()) {
        won = true;
      } else if (
        guess.toLowerCase() == death.death.split(" ")[0].toLowerCase()
      ) {
        won = true;
      }
    }
  }

  fetchAllDeaths();
</script>

<main>
  {#if won == false && lost == false}
    <div class="top-bar">
      <span class="timer"><Countdown on:timeout={() => (lost = true)} /></span>
    </div>
    <h1>{death.last_words}</h1>

    <div>
      <button type="button" on:click={getNewDeath} class="btn btn-outline"
        >New</button
      >
    </div>

    <div>
      <input
        type="search"
        bind:value={guess}
        on:input={validateGuess}
        placeholder="Type here"
        class="input input-bordered input-info w-full max-w-xs"
        class:input-success={won}
      />
    </div>
  {:else if won == true}
    <h2>Correct</h2>
  {:else if lost == true}
    <h1>YOU LOST</h1>
  {:else}
    <h1>Internal Server Error</h1>
  {/if}
</main>

<style>
  main {
    height: 100vh;
    width: 100vh;
    display: flex;
    overflow: none;
    flex-flow: column nowrap;
    gap: 5%;
  }

  .top-bar {
    width: 100vh;
    display: flex;
    justify-content: space-between;
    flex: 0.125;
  }

  .timer {
    flex: 1;
  }

  input {
    border-radius: 10px;
    border: 2px solid #d4d4d4;
    font-size: large;
    width: max-content;
  }

  h1 {
    flex: 0.25 1 auto;
    overflow: auto;
    font-family: "Courier New", Courier, monospace;
    font-weight: 500;
    min-height: 250px;
  }
</style>
