<script>
  // Import components
  import Question from "./Question.svelte";
  import Scoreboard from "./Scoreboard.svelte";
  import { fly, fade } from "svelte/transition";

  // Fetch new questions
  const startOver = async () => {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&difficulty=easy&type=multiple"
    );
    const quiz = await res.json();
    current = 0;
    score = 0;
    answered = false;
    questions = quiz.results;
  };

  const outcome = isCorrect => {
    if (isCorrect.detail === true) {
      score++;
      isCorrectAnswer = true;
    } else {
      isCorrectAnswer = false;
    }
    answered = true;
  };

  const nextQuestion = () => {
    answered = false;
    current++;
  };

  // Wait and assign new questions
  let current = 0;
  let questions = startOver(); // array with questions
  let score = 0;
  let answered = false;
  let isCorrectAnswer;

  $: question = questions[current];
</script>

<style>
  :global(:root) {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    width: 100%;
    height: fit-content;
  }

  :global(body) {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
    width: 100%;
  }

  .fade-wrapper {
    display: flex;
    flex-direction: column;
    align-items: center;
    position: absolute;
    z-index: 1;
    width: 100%;
  }

  main {
    padding: 1rem;
  }

  :global(button) {
    width: 20vw;
    border: 2px solid aqua;
    border-radius: 5px;
    background-color: white;
    margin: 0;
    padding: 5px 15px;
  }

  .correct {
    color: green;
  }

  .incorrect {
    color: red;
  }
</style>

<main>
  <h1>Trivia</h1>
  <Scoreboard {current} {score} />

  {#await questions}
    <p>Loading questions...</p>
  {:then questions}
    {#if current < 10}
      {#if answered}
        <div transition:fade class="fade-wrapper">

          {#if isCorrectAnswer}
            <h5 class="correct">Yaaay you got it</h5>
          {:else}
            <h5 class="incorrect">Boohoo not this time</h5>
            <p>Correct answer was: {question.correct_answer}</p>
          {/if}

          <button on:click={nextQuestion}>Next question</button>
        </div>
      {:else}
        <div in:fly={{ y: 100 }} out:fly={{ y: -200 }} class="fade-wrapper">
          <Question on:choice={isCorrect => outcome(isCorrect)} {question} />
        </div>
      {/if}
    {:else}
      <button on:click={startOver}>Start over</button>
    {/if}
  {/await}
</main>
