<script>
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  export let categories = [];
  export let API;
  import { createEventDispatcher } from "svelte";
	import { onMount } from 'svelte';

  let dispatch = createEventDispatcher();

  const deleteCat = async (id) => {
    try {
      await fetch(`${API}/categories/${id}`, {
        method: 'DELETE',
      });
      dispatch('DELETE', id);
    } catch (error) {
      console.log(error);
    }
  }
</script>

<Card>
  <h1>Categories:</h1>
  <table>
    <tr>
      <th>Name</th>
      <th>Options</th>
    </tr>
    {#each categories as category}
      <tr>
        <td>{category.name.toLowerCase()}</td>
        <td>
          <form on:submit|preventDefault={() => deleteCat(category._id)}>
            <Button type="primary">Delete Category</Button>
          </form>
        </td>
      </tr>
    {/each}
  </table>
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
</style>