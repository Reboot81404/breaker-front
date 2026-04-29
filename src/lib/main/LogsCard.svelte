<script>
  export let logs = [];
</script>

<div class="card">
  <h3>Loglar</h3>
  <pre>
{#each logs as line}
{line}
{/each}
  </pre>
</div>