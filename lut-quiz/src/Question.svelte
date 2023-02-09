<script>
  export let nextQuestion;
  export let question;
  export let incrementScore;
  let isCorrect;
  let isAnswered = false;
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

{#if isAnswered}
  <h5>
    {#if isCorrect}
      You got it right
    {:else}
      You goofed up
    {/if}
  </h5>
{/if}

{#each allAnswers as answer}
  <button on:click={() => checkQuestion(answer)}>
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}> Next Question </button>
  </div>
{/if}
