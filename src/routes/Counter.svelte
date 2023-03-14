<script lang="ts">
  import { spring } from "svelte/motion";

  let count = 0;

  const displayed_count = spring();
  $: displayed_count.set(count);
  $: offset = modulo($displayed_count, 1);

  function modulo(n: number, m: number) {
    // handle negative numbers
    return ((n % m) + m) % m;
  }
</script>

<div class="box-container">
  <div class="box" id="box-1" />
  <div class="box" id="box-2" />
  <div class="box" id="box-3" />
</div>
<div class="answer-container">
  <input class="answer-input" type="text" id="answer-input-1" />
  <input class="answer-input" type="text" id="answer-input-2" />
  <input class="answer-input" type="text" id="answer-input-3" />
  <input class="answer-input" type="text" id="answer-input-4" />
  <input class="answer-input" type="text" id="answer-input-5" />
  <input class="answer-input" type="text" id="answer-input-6" />
  <input class="answer-input" type="text" id="answer-input-7" />
  <input class="answer-input" type="text" id="answer-input-8" />
  <input class="answer-input" type="text" id="answer-input-9" />
</div>

<div class="counter">
  <button
    on:click={() => (count -= 1)}
    aria-label="Decrease the counter by one"
  >
    <svg aria-hidden="true" viewBox="0 0 1 1">
      <path d="M0,0.5 L1,0.5" />
    </svg>
  </button>

  <div class="counter-viewport">
    <div
      class="counter-digits"
      style="transform: translate(0, {100 * offset}%)"
    >
      <strong class="hidden" aria-hidden="true"
        >{Math.floor($displayed_count + 1)}</strong
      >
      <strong>{Math.floor($displayed_count)}</strong>
    </div>
  </div>

  <button
    on:click={() => (count += 1)}
    aria-label="Increase the counter by one"
  >
    <svg aria-hidden="true" viewBox="0 0 1 1">
      <path d="M0,0.5 L1,0.5 M0.5,0 L0.5,1" />
    </svg>
  </button>
</div>

<style>
  .box-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
  }
  .box {
    width: 140px;
    height: 190px;
    background: #eee;
    border-radius: 10px;
    margin: 0 10px;
  }

  .answer-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    margin-top: 6rem;
  }

  .answer-input {
    width: 120px;
    height: 170px;
    border: 5px solid #fff;
    color: #fff;
    background-color: transparent;
    border-radius: 10px;
    margin: 0 10px;
    text-align: center;
    font-size: 8rem;
  }

  .answer-input:focus {
    outline: none;
  }
</style>
