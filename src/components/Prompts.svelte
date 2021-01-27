<script>
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  import { createEventDispatcher, onMount } from "svelte";
  import Link from "svelte-routing/src/Link.svelte";
  
  let prompts = [];
  let dispatch = createEventDispatcher();

  let API = 'https://stormy-fortress-52595.herokuapp.com/api';

  onMount(async () => {
    const res = await fetch(`${API}/prompts`);
    prompts = await res.json();
  })

  const deletePhrase = async (id) => {
    try {
      await fetch(`${API}/prompts/${id}`, {
        method: 'DELETE',
      });
      dispatch('DELETE', id);
    } catch (error) {
      console.log(error);
    }
  }
</script>

<Card>
  <h1>Prompts:</h1>
  <table>
    <tr>
      <th>Body</th>
      <th>Options</th>
    </tr>
    {#each prompts as prompt}
      <tr>
      <Link to="/prompts/{prompt._id}"><td>{prompt.body}</td></Link>
        <td>
          <form on:submit|preventDefault={() => deletePhrase(prompt._id)}>
            <Button type="primary">Delete Prompt</Button>
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