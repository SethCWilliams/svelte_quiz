<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import Question from "./Question.svelte";

  let quiz = getQuiz();
  let activeQuestion = 0;
  let score = 0;

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10");
    // const quiz = await res.json();
    return await res.json();
  }

  function resetQuiz() {
    score = 0;
    activeQuestion = 0;
    quiz = getQuiz();
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function incrementScore() {
    score++;
  }

  // Reactive Statement
  $: if (score > 6) {
    alert("You Won!");
    resetQuiz();
  }
  // Reactive Declaration
  $: questionNumber = activeQuestion + 1;
</script>

<div>
  <button on:click={resetQuiz}> Start New Quiz </button>

  <h3>My Score: {score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      <!-- index isn't a keyword - second parameter keeps count -->
      {#if index == activeQuestion}
        <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fly-wrapper">
          <Question {nextQuestion} {incrementScore} {question} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

<style>
  .fly-wrapper {
    position: absolute;
  }
</style>
