<script>
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  export let phrases = [];
  export let API;
  import { createEventDispatcher } from "svelte";

  let dispatch = createEventDispatcher();

  const deletePhrase = async (id) => {
    try {
      await fetch(`${API}/phrases/${id}`, {
        method: 'DELETE',
      });
      dispatch('DELETE', id);
    } catch (error) {
      console.log(error);
    }
  }
</script>

<Card>
  <h1>Phrases:</h1>
  <table>
    <tr>
      <th>Body</th>
      <th>Options</th>
    </tr>
    {#each phrases as phrase}
      <tr>
      <td>{phrase.direction === 'left' ? '___' : ''} {phrase.body} {phrase.direction === 'right' ? '___' : ''}</td>
        <td>
          <form on:submit|preventDefault={() => deletePhrase(phrase._id)}>
            <Button type="primary">Delete Phrase</Button>
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