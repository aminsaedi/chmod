<script lang="ts">
  import { onMount } from "svelte";

  let question: [number, number, number] = [7, 7, 7];
  let inputs: string[] = Array(9).fill("?");
  let score = 0;

  function generateQuestion(): [number, number, number] {
    // select a random number between 1 and 7
    const first = Math.floor(Math.random() * 7) + 1;
    const second = Math.floor(Math.random() * 7) + 1;
    const third = Math.floor(Math.random() * 7) + 1;

    return [first, second, third];
  }

  function octalToStringMode(octal) {
    const permissions = [
      ["---", "--x", "-w-", "-wx", "r--", "r-x", "rw-", "rwx"], // User
      ["---", "--x", "-w-", "-wx", "r--", "r-x", "rw-", "rwx"], // Group
      ["---", "--x", "-w-", "-wx", "r--", "r-x", "rw-", "rwx"], // Other
    ];
    const digitToPermission = (digit, index) => permissions[index][digit];
    const mode = octal.toString().padStart(3, "0");
    const digits = mode.split("").map((digit) => +digit);
    const user = digitToPermission(digits[0], 0);
    const group = digitToPermission(digits[1], 1);
    const other = digitToPermission(digits[2], 2);
    return `${user}${group}${other}`;
  }

  onMount(() => {
    question = generateQuestion();
  });

  function rotate() {
    const first = question[0];
    const second = question[1];
    const third = question[2];

    question = [third, first, second];
  }

  $: {
    if (inputs.every((input) => input?.length > 0 && input !== "?")) {
      const answer = inputs.join("");

      const correct = octalToStringMode(Number(question.join("")));

      if (answer === correct) {
        alert("Correct!");
        inputs = Array(9).fill("?");
        score += Number(question.join(""));
        question = generateQuestion();
      } else {
        alert("Wrong!");
      }
    }
  }
</script>

<div class="score-container">
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <i class="fa-solid fa-question clickable" on:click={() => alert("Help")} />
  <div class="score-text">
    Score:
    <span>{score}</span>
  </div>
  <!-- svelte-ignore a11y-click-events-have-key-events -->
  <i class="fa-solid fa-rotate clickable" on:click={rotate} />
</div>
<div class="box-container">
  {#each question as box, index}
    <div class="box" id={`box-${index + 1}`}>{box}</div>
  {/each}
</div>
<div class="answer-container">
  {#each Array(9) as _, input}
    <input
      class="answer-input"
      id="answer-input-{input}"
      bind:value={inputs[input]}
    />
  {/each}
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
    color: #000;
    font-size: 10rem;
    text-align: center;
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

  .score-container {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 2rem;
    gap: 1rem;
    color: #fff;
    font-size: 3rem;
  }

  .score-text {
    font-size: 5rem;
  }

  .clickable {
    cursor: pointer;
  }
</style>
