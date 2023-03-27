<script lang="ts">
  import { onMount } from "svelte";

  let question: string[] = Array(9).fill("?");
  let inputs: string[] = Array(3).fill("?");
  let score = 0;

  function generateQuestion(): string[] {
    // select a random character from r w x
    const user_read = ["r", "-"][Math.floor(Math.random() * 2)];
    const user_write = ["w", "-"][Math.floor(Math.random() * 2)];
    const user_execute = ["x", "-"][Math.floor(Math.random() * 2)];

    const group_read = ["r", "-"][Math.floor(Math.random() * 2)];
    const group_write = ["w", "-"][Math.floor(Math.random() * 2)];
    const group_execute = ["x", "-"][Math.floor(Math.random() * 2)];

    const other_read = ["r", "-"][Math.floor(Math.random() * 2)];
    const other_write = ["w", "-"][Math.floor(Math.random() * 2)];
    const other_execute = ["x", "-"][Math.floor(Math.random() * 2)];

    return [
      user_read,
      user_write,
      user_execute,
      group_read,
      group_write,
      group_execute,
      other_read,
      other_write,
      other_execute,
    ];
  }

  function octalToStringMode(octal: number) {
    const permissions = [
      ["---", "--x", "-w-", "-wx", "r--", "r-x", "rw-", "rwx"], // User
      ["---", "--x", "-w-", "-wx", "r--", "r-x", "rw-", "rwx"], // Group
      ["---", "--x", "-w-", "-wx", "r--", "r-x", "rw-", "rwx"], // Other
    ];
    const digitToPermission = (digit: number, index: number): string =>
      permissions[index][digit];
    const mode = octal.toString().padStart(3, "0");
    const digits = mode.split("").map((digit: string) => +digit);
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
        inputs = Array(3).fill("?");
        score += Number(question.join(""));
        question = generateQuestion();
      }
    }
  }
</script>

<svelte:head>
  <title>Charachter Mode</title>
  <meta name="description" content="Chmod game - charachter mode" />
</svelte:head>

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
  {#each Array(3) as _, input}
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
    min-width: 140px;
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
    min-width: 120px;
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
