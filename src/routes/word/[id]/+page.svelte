<script>
  import { page } from '$app/stores';
  import { goto } from '$app/navigation';
  import PipelineViz from '$lib/components/PipelineViz.svelte';
  import NearestWords from '$lib/components/NearestWords.svelte';

  // Извлекаем id из URL
  let id;
  $: id = $page.params.id;

  // Моковые детали
  const mockDetails = [
    {
      id: '1',
      term: 'арбуз',
      definition: 'Крупная сочная ягода с толстой коркой.',
      pipeline: ['арбуз!!??', 'арбуз', 'арбуз'],
      neighbors: [
        { id: '2', term: 'абрикос' },
        { id: '3', term: 'автомобиль' }
      ]
    },
    {
      id: '2',
      term: 'абрикос',
      definition: 'Сочная косточковая плодовая культура.',
      pipeline: ['... абрикос ...', 'абрикос', 'абрикос'],
      neighbors: [
        { id: '1', term: 'арбуз' },
        { id: '6', term: 'баран' }
      ]
    }
    // … другие записи
  ];

  // Находим детальную инфу
  let detail = mockDetails.find(d => d.id === id) || {
    id,
    term: 'Слово не найдено',
    definition: '',
    pipeline: [],
    neighbors: []
  };

  function handleEdit() {
    goto(`/add?id=${detail.id}`);
  }

  function handleDelete() {
    if (confirm(`Удалить слово «${detail.term}»?`)) {
      // здесь будет вызов API удаления
      alert(`Слово «${detail.term}» удалено.`);
      goto('/');
    }
  }
</script>

<article class="detail-page">
  <h2 class="term">{detail.term}</h2>
  {#if detail.definition}
    <p class="definition">{detail.definition}</p>
  {/if}

  <PipelineViz steps={detail.pipeline} />

  <NearestWords neighbors={detail.neighbors} />

  <div class="actions">
    <button class="edit" on:click={handleEdit}>Редактировать</button>
    <button class="delete" on:click={handleDelete}>Удалить</button>
  </div>
</article>

<style>
  .detail-page {
    padding: 1rem 0;
  }

  .term {
    font-size: 2rem;
    margin: 0;
  }

  .definition {
    margin: 0.5rem 0 1.5rem;
    font-size: 1.1rem;
    color: #333;
  }

  .actions {
    display: flex;
    gap: 1rem;
    margin-top: 2rem;
  }

  .actions button {
    padding: 0.6rem 1.2rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.2s;
  }

  .actions button.edit {
    background-color: #0077cc;
    color: #fff;
  }

  .actions button.edit:hover {
    background-color: #005fa3;
  }

  .actions button.delete {
    background-color: #cc0000;
    color: #fff;
  }

  .actions button.delete:hover {
    background-color: #a30000;
  }
</style>
