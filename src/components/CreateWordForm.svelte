<script>
  import { createEventDispatcher } from "svelte";
  import Button from "../shared/Button.svelte";
  export let categories = [];

  let dispatch = createEventDispatcher();
  let fields = { word: "", type: "", category: "" };
  let errors = { word: "", type: "", category: "" };
  let valid = false;

  const handleSubmit = () => {
    valid = true;

    if (fields.word.trim().length < 1) {
      valid = false;
      errors.word = "Must include a word.";
    } else {
      errors.word = "";
    }

    if (fields.type.trim().length < 1) {
      valid = false;
      errors.type = "Must include a type.";
    } else {
      errors.type = "";
    }

    if (fields.category.trim().length < 1) {
      valid = false;
      errors.category = "Must include a category.";
    } else {
      errors.category = "";
    }

    if (valid) {
      let newWord = { ...fields };
      fields = { word: "", type: "", category: "" };
      dispatch("add", newWord);
    }
  };
</script>

<style>
  form {
    width: 400px;
    margin: 0 auto;
    text-align: left;
  }

  .form-field {
    margin: 18px auto;
  }

  input {
    width: 100%;
    border-radius: 6px;
  }

  label {
    margin: 10px auto;
    text-align: left;
  }

  .error {
    font-weight: bold;
    font-size: 12px;
    color: #d91b42;
  }
</style>

<form on:submit|preventDefault={handleSubmit}>
  <div class="form-field">
    <label for="word">Word:</label>
    <input type="text" id="word" bind:value={fields.word} />
    <div class="error">{errors.word}</div>
  </div>
  <div class="form-field">
    <label for="type">Type:</label>
    <select id="type" bind:value={fields.type}>
      <option value="">Select Type</option>
      <option value="noun">Noun</option>
      <option value="proper noun">Proper Noun</option>
      <option value="pronoun">Pronoun</option>
      <option value="verb">Verb</option>
      <option value="adjective">Adjective</option>
      <option value="adverb">Adverb</option>
      <option value="action verb">Action Verb</option>
      <option value="linking verb">Linking Verb</option>
    </select>
    <div class="error">{errors.type}</div>
  </div>
  <div class="form-field">
    <label for="category">Category:</label>
    <select id="category" bind:value={fields.category}>
      <option value="">Select Category</option>
      {#each categories as category(category._id)}
        <option value={category._id}>{category.name.toLowerCase()}</option>
      {/each}
    </select>
    <div class="error">{errors.category}</div>
  </div>
  <Button type="secondary" flat={true}>Add Word</Button>
</form>
