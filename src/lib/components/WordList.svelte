<script>
  export let words = [];

  // Группируем по первой букве
  $: grouped = words.reduce((acc, w) => {
    const letter = w.term[0].toUpperCase();
    (acc[letter] ||= []).push(w);
    return acc;
  }, {});

  // Сортируем и внутри групп
  $: sortedGroups = Object.keys(grouped)
    .sort()
    .reduce((obj, key) => {
      obj[key] = grouped[key].sort((a, b) =>
        a.term.localeCompare(b.term)
      );
      return obj;
    }, {});
</script>

{#if words.length === 0}
  <p>Нет слов.</p>
{:else}
  {#each Object.entries(sortedGroups) as [letter, items]}
    <div class="group">
      <h3>{letter}</h3>
      <ul>
        {#each items as w}
          <li><a href={`/word/${w.id}`}>{w.term}</a></li>
        {/each}
      </ul>
    </div>
  {/each}
{/if}

<style>
  .group {
    margin-bottom: 1rem;
  }
  h3 {
    margin: 0.5rem 0;
    font-weight: bold;
  }
  ul {
    list-style: none;
    padding: 0;
    margin: 0;
  }
  li {
    margin-left: 1rem;
  }
  a {
    text-decoration: none;
    color: #0077cc;
  }
  a:hover {
    text-decoration: underline;
  }
</style>
