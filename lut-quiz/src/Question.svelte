<script>
  export let question;

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
  console.log(allAnswers);

  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  function checkQuestion(answer) {
    isAnswered = true;
    isCorrect = answer.correct;
  }
</script>

<h3>{@html question.question}</h3>

{#if isAnswered}
  <h4>
    {#if isCorrect}
      You got it right
    {:else}
      You goofed up
    {/if}
  </h4>
{/if}

{#each allAnswers as answer}
  <button on:click={() => checkQuestion(answer)}>
    {@html answer.answer}
  </button>
{/each}
