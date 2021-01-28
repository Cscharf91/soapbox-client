<script>
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  import { onMount } from "svelte";
  import Link from "svelte-routing/src/Link.svelte";
  import axios from "axios";
  import { words, categories } from "../Stores.js";
  import CreateSoapstoneForm from "./CreateSoapstoneForm.svelte";

  export let id;
  let prompt;
  let soapstones = [];

  let API = "https://stormy-fortress-52595.herokuapp.com/api";

  onMount(async () => {
    const { data } = await axios.get(`${API}/prompts/${id}`);
    prompt = data.prompt;
    soapstones = data.soapstones;
    // const res = await fetch(`${API}/prompts`);
    // prompts = await res.json();
  });

  const deleteSoapstone = async (deletedId) => {
    try {
      await axios.delete(`${API}/soapstones/${deletedId}`);
      soapstones = soapstones.filter(
        (soapstone) => soapstone._id !== deletedId
      );
    } catch (error) {
      console.log(error);
    }
  };

  const handleAdd = async (e) => {
    const { body, nickname } = e.detail
    const newSoapstone = { body, nickname, prompt: id };
    try {
      const { data } = await axios.post(`${API}/soapstones`, newSoapstone);
      soapstones = [data, ...soapstones];
    } catch (err) {
      console.log(err);
    }
  };
</script>

<style>
  h1 {
    text-align: center;
  }

  table {
    border-collapse: collapse;
    width: 100%;
  }

  td,
  th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
    width: 100%;
  }

  tr:nth-child(even) {
    background-color: #dddddd;
  }
</style>

{#if prompt}
  <h1>Prompt:</h1>
  <h1>{prompt.body}</h1>
{/if}
<CreateSoapstoneForm on:add={handleAdd} />
<Card>
  <table>
    <tr>
      <th>Body</th>
      <th>Options</th>
    </tr>
    {#each soapstones as soapstone (soapstone._id)}
      <tr>
        <td>{soapstone.body}</td>
        <td>
          <form on:submit|preventDefault={() => deleteSoapstone(soapstone._id)}>
            <Button type="primary">Delete Prompt</Button>
          </form>
        </td>
      </tr>
    {/each}
  </table>
</Card>
