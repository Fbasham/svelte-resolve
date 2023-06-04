<script>
  let options = [];
  let index = 0;

  function handleSubmit(e) {
    let option = e.target.option.value.trim();
    if (option && !options.some((o) => o.option === option)) {
      options = [...options, { id: +new Date(), option }];
    }
    e.target.option.value = "";
  }

  function spin(delay = 300) {
    let duration = 2000 + Math.random() * 2000;
    let interval = setInterval(() => {
      duration -= delay;
      index = (index + 1) % options.length;
      console.log(index, duration, delay);
      if (duration <= 0) clearInterval(interval);
    }, delay);
  }
</script>

<h1>Resolve</h1>
<form on:submit|preventDefault={handleSubmit}>
  <label>
    Option:
    <input name="option" />
  </label>
</form>
<button on:click={() => spin()}>Spin</button>
<div class="options">
  {#each options as { id, option }}
    <div class="option" class:active={options[index].id === id}>
      <p>{option}</p>
      <button on:click={() => (options = options.filter((o) => o.id !== id))}
        >&times;</button
      >
    </div>
  {/each}
</div>

<style>
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
  }
</style>
