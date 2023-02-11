<script>
  import { score } from "./stores.js";
  export let nextQuestion;
  export let question;

  // bool values for monitoring status
  let isCorrect;
  let isAnswered = false;

  // put answers in object list and shuffle them up
  let answer = question.correct_answer;
  let answers = question.incorrect_answers.map((answer) => {
    return {
      answer,
      correct: false,
    };
  });
  let allAnswers = [
    ...answers,
    { answer: question.correct_answer, correct: true },
  ];

  shuffle(allAnswers);
  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  // check for correct answer
  function checkQuestion(answer) {
    if (!isAnswered) {
      isAnswered = true;
      isCorrect = answer.correct;
      if (answer.correct) {
        score.update((n) => n + 1);
      }
    }
  }
</script>

<h3>{@html question.question}</h3>

{#each allAnswers as answer}
  <!-- <div> -->
  <button disabled={isAnswered} on:click={() => checkQuestion(answer)}>
    {@html answer.answer}
  </button>
  <!-- </div> -->
{/each}
{#if isAnswered}
  <h5 class:isCorrect class:wrong={!isCorrect}>
    {#if isCorrect}
      You got it right
    {:else}
      You goofed up. The answer is {answer}
    {/if}
  </h5>
{/if}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}> Next Question </button>
  </div>
{/if}

<style>
  .wrong {
    color: red;
  }
  .isCorrect {
    color: green;
  }
  button {
    display: block;
    margin: 5px;
    border-radius: 15px;
  }
</style>
