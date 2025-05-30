<script>
  import { onMount } from 'svelte';
  import { page } from '$app/stores';
  import { goto } from '$app/navigation';

  // Значения полей
  let term = '';
  let definition = '';

  // Если пришёл параметр ?id=… — мы в режиме редактирования
  let id;
  $: id = $page.url.searchParams.get('id');

  // Заглушка данных для редактирования
  const mockDetails = [
    { id: '1', term: 'арбуз', definition: 'Крупная сочная ягода с толстой коркой.' },
    { id: '2', term: 'абрикос', definition: 'Сочная косточковая плодовая культура.' }
    // …добавьте при необходимости
  ];

  onMount(() => {
    if (id) {
      const detail = mockDetails.find(d => d.id === id);
      if (detail) {
        term = detail.term;
        definition = detail.definition;
      }
    }
  });

  function handleSave() {
    if (!term.trim()) {
      alert('Пожалуйста, введите слово.');
      return;
    }
    // TODO: здесь будет вызов API для сохранения
    if (id) {
      alert(`Слово «${term}» обновлено.`);
      goto(`/word/${id}`);
    } else {
      alert(`Слово «${term}» добавлено.`);
      goto('/');
    }
  }
</script>

<article class="form-page">
  <h2>{id ? 'Редактировать слово' : 'Добавить слово'}</h2>

  <div class="field">
    <label for="term">Слово</label>
    <input
      id="term"
      type="text"
      bind:value={term}
      placeholder="Введите слово"
      autofocus
    />
  </div>

  <div class="field">
    <label for="definition">Описание</label>
    <textarea
      id="definition"
      bind:value={definition}
      placeholder="Введите описание"
    ></textarea>
  </div>

  <button class="save" on:click={handleSave}>
    Сохранить
  </button>
</article>

<style>
  .form-page {
    padding: 1rem 0;
  }

  h2 {
    margin-bottom: 1.5rem;
  }

  .field {
    margin-bottom: 1.25rem;
  }

  label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: bold;
  }

  input,
  textarea {
    width: 100%;
    padding: 0.5rem;
    font-size: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

  textarea {
    min-height: 8rem;
    resize: vertical;
  }

  .save {
    padding: 0.6rem 1.2rem;
    background-color: #0077cc;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.2s;
  }

  .save:hover {
    background-color: #005fa3;
  }
</style>
