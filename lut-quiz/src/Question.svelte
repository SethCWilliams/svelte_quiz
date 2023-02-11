<script>
  export let nextQuestion;
  export let question;
  export let incrementScore;

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
        incrementScore();
      }
    }
  }
</script>

<h3>{@html question.question}</h3>

{#each allAnswers as answer}
  <button disabled={isAnswered} on:click={() => checkQuestion(answer)}>
    {@html answer.answer}
  </button>
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
</style>
