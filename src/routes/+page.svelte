<!-- src/routes/+page.svelte -->
<script>
  import { goto } from '$app/navigation';
  import Tabs from '$lib/components/Tabs.svelte';
  import SearchBar from '$lib/components/SearchBar.svelte';
  import WordList from '$lib/components/WordList.svelte';

    import { placeholder } from '$lib/stores/searchPlaceholder';

  const topics = ['Физический', 'Химический', 'Физико-химический'];
  let selected = topics[0];
  let search = '';

  // Моковые данные
  const mockWords = [
    { id: '1', term: 'арбуз', topic: 'Физический' },
    { id: '2', term: 'абрикос', topic: 'Физический' },
    { id: '3', term: 'автомобиль', topic: 'Физический' },
    { id: '4', term: 'батинок', topic: 'Физический' },
    { id: '5', term: 'балалайка', topic: 'Физический' },
    { id: '6', term: 'баран', topic: 'Физический' },
    { id: '7', term: 'кислота', topic: 'Химический' },
    { id: '8', term: 'основание', topic: 'Химический' },
    { id: '9', term: 'соль', topic: 'Химический' },
    { id: '10', term: 'смесь', topic: 'Физико-химический' },
    { id: '11', term: 'раствор', topic: 'Физико-химический' },
    { id: '12', term: 'суспензия', topic: 'Физико-химический' }
  ];

  // Фильтрация “на лету”
  $: filtered = mockWords.filter(
    w =>
      w.topic === selected &&
      w.term.toLowerCase().includes(search.trim().toLowerCase())
  );

    function selectTab(t) {
    selected = t;
    search = '';
    placeholder.set('');
  }
</script>

<!-- Передаём переменную topics как tabs -->
<Tabs
  tabs={topics}
  selected={selected}
  onSelect={t => {
    selected = t;
    search = '';
  }}
/>

<div class="controls">
  <SearchBar
    value={search}
    placeholder={$placeholder || 'Поиск...'}
    onInput={val => (search = val)}
  />
  <button class="add-button" on:click={() => goto('/add')}>
    Добавить
  </button>
</div>

<WordList words={filtered} />

<style>
  .controls {
    display: flex;
    gap: 1rem;
    margin-bottom: 1rem;
  }
  .add-button {
    padding: 0.5rem 1rem;
    background-color: #0077cc;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }
  .add-button:hover {
    background-color: #005fa3;
  }
</style>
