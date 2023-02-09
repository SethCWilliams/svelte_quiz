<script>
  import Question from "./Question.svelte";

  let quiz = getQuiz();

  async function getQuiz() {
    const res = await fetch("https://opentdb.com/api.php?amount=10");
    // const quiz = await res.json();
    return await res.json();
  }

  function handleClick() {
    quiz = getQuiz();
  }
</script>

<div>
  <button on:click={handleClick}> Get Questions </button>

  {#await quiz}
    Loading...
  {:then data}
    {#each data.results as question}
      <Question {question} />
    {/each}
  {/await}
</div>
