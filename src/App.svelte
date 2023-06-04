<script>
  let options = [];
  let index = -1;
  let spinning = false;
  let winner;

  function handleSubmit(e) {
    let option = e.target.option.value.trim();
    if (option && !options.some((o) => o.option === option)) {
      options = [...options, { id: +new Date(), option }];
    }
    e.target.option.value = "";
  }

  function spin() {
    let delay = 150;
    let duration = 2000 + Math.random() * options.length * 1000;
    let interval = setInterval(() => {
      spinning = true;
      index = (index + 1) % options.length;
      duration -= delay;
      console.log(spinning, index, duration, delay);
      if (duration <= 0) {
        winner = options[index];
        spinning = false;
        clearInterval(interval);
        return;
      }
    }, delay);
  }
</script>

<h1>
  R<span style="color:darkorchid">e</span>s<span style="color:darkorchid"
    >o</span
  >l<span style="color:darkorchid">v</span>e
</h1>
<form on:submit|preventDefault={handleSubmit}>
  <label>
    Option:
    <input name="option" />
  </label>
</form>
<button
  class="spinBtn"
  on:click={spin}
  disabled={spinning || options.length < 1}>Spin</button
>
{#if winner}
  <p class="winText">
    <strong>Winner: </strong><span style="color:darkorchid"
      >{winner.option}</span
    >
  </p>
{/if}

<div class="options">
  {#each options as { id, option }}
    <div
      class="option"
      class:winner={!spinning && winner && winner.id === id}
      class:active={(!spinning && id === winner?.id) ||
        (spinning && options[index]?.id === id)}
    >
      <p>{option}</p>
      <button
        hidden={spinning}
        disabled={spinning}
        on:click={() => (options = options.filter((o) => o.id !== id))}
        >&times;</button
      >
    </div>
  {/each}
</div>

<style>
  h1 {
    margin-bottom: 1rem;
    font-size: 2.5rem;
  }
  h1 span {
    font-size: 2.7rem;
  }
  label {
    font-size: 1.1rem;
  }
  input {
    margin-bottom: 1rem;
    margin-left: 0.5rem;
    padding: 2px;
    border: 2px solid darkorchid;
    border-radius: 5px;
  }
  .options {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-top: 1rem;
  }
  .option {
    position: relative;
    padding: 0.5rem;
    background-color: darkorchid;
    color: white;
    transition: 100ms;
  }
  .option button {
    cursor: pointer;
    position: absolute;
    top: -7.5px;
    right: -7.5px;
    background-color: rgb(245, 0, 0);
    border: 2px solid white;
    color: white;
    border-radius: 50%;
    height: 25px;
    width: 25px;
  }
  .option button:hover {
    background-color: rgb(200, 0, 0);
  }
  .active {
    background-color: yellow;
    color: black;
    transition: 100ms;
  }
  .spinBtn {
    display: inline;
    cursor: pointer;
    border: 2px solid darkorchid;
    padding: 0.5rem 1rem;
    border-radius: 5px;
    background-color: transparent;
    color: darkorchid;
    transition: 200ms;
  }
  .spinBtn:hover {
    background-color: darkorchid;
    color: white;
  }
  .winner {
    background-color: darkgreen;
    color: white;
    transition: 100ms;
  }
  .winText {
    display: inline;
    margin-left: 1rem;
    font-size: 1.1rem;
  }
</style>
