<script>
  import { createEventDispatcher } from "svelte";
  import Button from "../shared/Button.svelte";
  import Card from "../shared/Card.svelte";
  import { words, phrases, categories, prompts } from '../Stores.js';

  let conjunctions = [
    "and",
    "but",
    "and then",
    "therefore",
    "or",
    // ",",
    "only",
    "by the way",
    "in short",
  ];

  let filter1 = '';
  let filter2 = '';
  $: filteredWords1 = !filter1 ? [...$words] : $words.filter(word => word.category._id === filter1);
  $: filteredWords2 = !filter2 ? [...$words] : $words.filter(word => word.category._id === filter2);

  let dispatch = createEventDispatcher();
  let fields = {
    // creator: "",
    phrase1: "",
    word1: "",
    conjunction: "",
    phrase2: "",
    word2: "",
  };
  let errors = {
    phrase1: "",
    word1: "",
    conjunction: "",
    phrase2: "",
    word2: "",
  };
  let valid = false;

  const handleSubmit = () => {
    valid = true;

    if (!fields.word1.word) {
      valid = false;
      errors.word1 = "Must have a word.";
    } else {
      errors.word1 = "";
    }

    if (!fields.phrase1.body) {
      valid = false;
      errors.phrase1 = "Must have a phrase.";
    } else {
      errors.phrase1 = "";
    }

    if (valid) {
      const newSentence = `${fields.phrase1.direction === 'left' ? fields.word1.word || "***" : fields.phrase1 && fields.phrase1.body} ${fields.phrase1.direction === 'right' ? fields.word1.word || "***" : fields.phrase1 && fields.phrase1.body} ${fields.conjunction && fields.conjunction} ${fields.phrase2.direction === 'left' ? fields.word2.word || "***" : fields.phrase2 && fields.phrase2.body} ${fields.phrase2.direction === 'right' ? fields.word2.word || "***" : fields.phrase2 && fields.phrase2.body}`;
      dispatch("add", newSentence);
    }
  };
</script>

<style>
  form {
    width: 400px;
    margin: 0 auto;
    text-align: left;
  }

  .form-field, select {
    margin: 18px auto;
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

<Card>
  <form on:submit|preventDefault={handleSubmit}>
    <h1>
      {fields.phrase1.direction === 'left' ? fields.word1.word || "***" : fields.phrase1 && fields.phrase1.body}
      {fields.phrase1.direction === 'right' ? fields.word1.word || "***" : fields.phrase1 && fields.phrase1.body}<br/>{fields.conjunction && fields.conjunction === ', ' ? `${fields.conjunction}` : ` ${fields.conjunction} `}
      {fields.phrase2.direction === 'left' ? fields.word2.word || "***" : fields.phrase2 && fields.phrase2.body}
      {fields.phrase2.direction === 'right' ? fields.word2.word || "***" : fields.phrase2 && fields.phrase2.body}
    </h1>
    <div class="form-field">
      <label for="phrase1">Phrase:</label>
      <select id="phrase1" bind:value={fields.phrase1}>
        <option value="">Select Phrase</option>
        {#each $phrases as phrase(phrase._id)}
          <option id={phrases._id} value={phrase}>{phrase.direction === 'left' ? "***" : ""} {phrase.body} {phrase.direction === 'right' ? "***" : ""}</option>
        {/each}
      </select>
      <div class="error">{errors.phrase1}</div>
      <label for="filter1">Category:</label>
      <select bind:value={filter1}>
        <option value="">None</option>
        {#each $categories as category(category._id)}
          <option value={category._id}>{category.name}</option>
        {/each}
      </select>
      <label for="word1">Word:</label>
      <select id="word1" bind:value={fields.word1}>
        <option value="">Select Word</option>
        {#each filteredWords1 as word(word._id)}
          <option id={word._id} value={word}>{word.word}</option>
        {/each}
      </select>
      <div class="error">{errors.word1}</div>
      
      <label for="conjunction">Conjunction:</label>
      <select id="conjunction" bind:value={fields.conjunction}>
        <option value="">Select Conjunction</option>
        {#each conjunctions as mid}
          <option value={mid}>{mid}</option>
        {/each}
      </select><br>
  
      <label for="phrase2">Phrase 2:</label>
      <select id="phrase2" bind:value={fields.phrase2}>
        <option value="">Select Phrase</option>
        {#each $phrases as phrase(phrase._id)}
          <option id={phrases._id} value={phrase}>{phrase.direction === 'left' ? "***" : ""} {phrase.body} {phrase.direction === 'right' ? "***" : ""}</option>
        {/each}
      </select><br>
      <label for="filter2">Category:</label>
      <select bind:value={filter2}>
        <option value="">None</option>
        {#each $categories as category(category._id)}
          <option value={category._id}>{category.name}</option>
        {/each}
      </select>
      <label for="word2">Word:</label>
      <select id="word2" bind:value={fields.word2}>
        <option value="">Select Word</option>
        {#each filteredWords2 as word(word._id)}
          <option id={word._id} value={word}>{word.word}</option>
        {/each}
      </select>
    </div>
    <Button type="secondary" flat={true}>Submit</Button>
  </form>  
</Card>
