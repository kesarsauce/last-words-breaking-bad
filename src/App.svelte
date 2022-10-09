<script>
  let death = {last_words: ""}
  let guess = "";
  let won = false;
  let out = [];

  async function fetchAllDeaths() {
    won = false;
    let response = await fetch("https://www.breakingbadapi.com/api/deaths");
    out = await response.json()
    out = out.filter(filterDeaths)
  }

  function getNewDeath() {
    const random = Math.floor(Math.random() * out.length);
    death = out[random]
  }

  function filterDeaths(death){
    let invalidLastWords = ["", "unknown", "unknwon"]
    if (invalidLastWords.includes(death.last_words.toLowerCase())){
      return false
    }
    if (death.death.toLowerCase() == "declan"){
      return false
    }

    return true
  }

  function validateGuess(){
    if (guess != undefined) {
      if (guess.toLowerCase() == death.death.toLowerCase()){
      won = true;
      }
      else if (guess.toLowerCase() == death.death.split(" ")[0].toLowerCase()){
        won = true;
      }
    }
  }

  fetchAllDeaths();
</script>

<button on:click={getNewDeath}>New</button>

<h1>{death.last_words}</h1>

<input bind:value={guess} on:input={validateGuess}>

{#if won == true}
  <h2>Correct</h2>
{/if}