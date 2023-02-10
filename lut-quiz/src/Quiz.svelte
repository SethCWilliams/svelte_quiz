<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Question from "./Question.svelte";
  import Modal from "./Modal.svelte";

  let quiz = getQuiz();
  let activeQuestion = 0;
  let score = 0;
  let isModalOpen = false;

  // component based life cycle methods
  onMount(() => {
    console.log("i mounted");
  });
  afterUpdate(() => {
    console.log("after update");
  });
  beforeUpdate(() => {
    console.log("before update");
  });

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10");
    // const quiz = await res.json();
    return await res.json();
  }

  function resetQuiz() {
    score = 0;
    activeQuestion = 0;
    isModalOpen = false;
    quiz = getQuiz();
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  function incrementScore() {
    score++;
  }

  // Reactive Statement
  $: if (score > 2) {
    isModalOpen = true;
  }
  // Reactive Declaration
  $: questionNumber = activeQuestion + 1;
</script>

<div>
  <!--could use on:click | once to make it so the button can be clicked once per mount-->
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

{#if isModalOpen}
  <Modal>
    <h2>You Won!</h2>
    <p>Congratulations!</p>
    <button on:click={resetQuiz}>Start New Quiz</button>
  </Modal>
{/if}

<style>
  .fly-wrapper {
    position: absolute;
  }
</style>
