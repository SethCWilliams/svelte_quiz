<script>
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
  import Question from "./Question.svelte";
  import Modal from "./Modal.svelte";
  import { score } from "./stores.js";

  let quiz = getQuiz();
  let activeQuestion = 0;
  let isModalOpen = false;

  // component based life cycle methods
  //   onMount(() => {
  //     console.log("i mounted");
  //   });
  //   afterUpdate(() => {
  //     console.log("after update");
  //   });
  //   beforeUpdate(() => {
  //     console.log("before update");
  //   });

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10");
    // const quiz = await res.json();
    return await res.json();
  }

  function resetQuiz() {
    score.update((n) => 0);
    activeQuestion = 0;
    isModalOpen = false;
    quiz = getQuiz();
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
  }

  // Reactive Statement
  $: if ($score > 1) {
    isModalOpen = true;
  }
  // Reactive Declaration
  $: questionNumber = activeQuestion + 1;
</script>

<div>
  <!--could use on:click | once to make it so the button can be clicked once per mount-->
  <button on:click={resetQuiz}> Start New Quiz </button>

  <h3>My Score: {$score}</h3>
  <h4>Question #{questionNumber}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      <!-- index isn't a keyword - second parameter keeps count -->
      {#if index == activeQuestion}
        <div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fly-wrapper">
          <Question {nextQuestion} {question} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

{#if isModalOpen}
  <!-- Modal has an 'on:close' event. close will be passed from dispatch in child component -->
  <Modal on:close={resetQuiz}>
    <h2>You Won!</h2>
    <p>Congratulations!</p>
    <button on:click={resetQuiz}>Start New Quiz</button>
  </Modal>
{/if}

<style>
  .fly-wrapper {
    position: absolute;
  }

  button {
    border-radius: 15px;
  }
</style>
