<script>
  import Card from "../shared/Card.svelte";
  export let API;
  import { createEventDispatcher } from "svelte";
  import Button from "../shared/Button.svelte";
  import { words, categories } from '../Stores.js';

  let filter = '';
  $: filteredWords = !filter ? [...$words] : $words.filter(word => word.category._id === filter);

  let dispatch = createEventDispatcher();

  const deleteWord = async (id) => {
    try {
      await fetch(`${API}/words/${id}`, {
        method: 'DELETE',
      });
      dispatch('DELETE', id);
    } catch (error) {
      console.log(error);
    }
  }
</script>

<Card>
  <h1>Words:</h1>
  {#if $words && $categories}
  <p>Filter by Category</p>
  <select bind:value={filter}>
    <option value="">None</option>
    {#each $categories as category}
      <option value={category._id}>{category.name}</option>
    {/each}
  </select>
  <table>
    <tr>
      <th>Word</th>
      <th>Type</th>
      <th>Category</th>
      <th>Options</th>
    </tr>
    {#each filteredWords as word}
      <tr>
        <td>{word.word}</td>
        <td>{word.type}</td>
        <td>{word.category.name}</td>
        <td>
          <form on:submit|preventDefault={() => deleteWord(word._id)}>
            <Button type="primary">Delete Word</Button>
          </form>
        </td>
      </tr>
    {/each}
  </table>
  {/if}
</Card>

<style>
  table {
    border-collapse: collapse;
    width: 100%;
  }

  td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
    width: 100%;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
  
  select {
    margin-bottom: 20px;
  }
</style>