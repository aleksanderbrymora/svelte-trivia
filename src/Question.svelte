<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte"; //Used to send custom events
  import { fly } from "svelte/transition";

  export let question;
  const dispatch = createEventDispatcher();

  $: answers = [
    { option: question.correct_answer, correct: true },
    ...question.incorrect_answers.map(a => {
      return { option: a, correct: false };
    })
  ].sort(() => Math.random() - 0.5);
</script>

<style lang="scss">
  section {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0 2rem;

    h5 {
      font-size: 200%;
      text-align: center;
    }

    .answers {
      display: grid;
      grid-template-columns: repeat(2, minmax(15vw, 1fr));
      grid-template-rows: 1fr 1fr;
      grid-gap: 0.5rem;

      button {
        width: 100%;
      }
    }
  }
</style>

<section>
  <h5>
    {@html question.question}
  </h5>
  <div class="answers">
    {#each answers as answer}
      <button on:click={() => dispatch('choice', answer.correct)}>
        {@html answer.option}
      </button>
    {/each}
  </div>
</section>
