<script>
  export let score, current;
</script>

<h3>Your score: {score}</h3>
{#if current < 10}
  <h4>Current question: #{current + 1}</h4>
{:else if score >= 7}
  <h5>You won</h5>
{:else}
  <h5>You lost :(</h5>
{/if}
