<script>
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

  function incrementScore() {
    score++;
  }

  function nextQuestion() {
    activeQuestion = activeQuestion + 1;
    console.log(activeQuestion);
  }
</script>

<div>
  <button on:click={resetQuiz}> Start New Quiz </button>

  <h3>My Score: {score}</h3>
  <h4>Question #{activeQuestion + 1}</h4>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question, index}
      <!-- index isn't a keyword - second parameter keeps count -->
      {#if index == activeQuestion}
        <Question {nextQuestion} {incrementScore} {question} />
      {/if}
    {/each}
  {/await}
</div>
