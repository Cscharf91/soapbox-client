<script>
  import { createEventDispatcher } from "svelte";
  import Button from "../shared/Button.svelte";
  export let phrases = [];
  export let words = [];
  // export let categories = [];
  let middleWords = [
    "and",
    "but",
    "and then",
    "therefore",
    "or",
    ",",
    "only",
    "by the way",
    "in short",
  ];

  let dispatch = createEventDispatcher();
  let fields = {
    phrase1: "",
    word1: "",
    middleWord: "",
    phrase2: "",
    word2: "",
  };
  let errors = {
    phrase1: "",
    word1: "",
    middleWord: "",
    phrase2: "",
    word2: "",
  };
  // let valid = false;

  const handleSubmit = () => {
    // valid = true;

    // if (fields.word1.length < 1) {
    //   valid = false;
    //   errors.body = "Must have a word.";
    // } else {
    //   errors.body = "";
    // }

    // if (valid) {
      let newSentence = { ...fields };
      fields = { phrase1: "", word1: "" };
      dispatch("add", newSentence);
    // }
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
  <h1>
    {fields.phrase1.direction === 'left' ? fields.word1 && fields.word1.word : fields.phrase1 && fields.phrase1.body}
    {fields.phrase1.direction === 'right' ? fields.word1 && fields.word1.word : fields.phrase1 && fields.phrase1.body}{fields.middleWord && fields.middleWord === ',' ? `${fields.middleWord}` : ` ${fields.middleWord}`}
    {fields.phrase2.direction === 'left' ? fields.word2 && fields.word2.word : fields.phrase2 && fields.phrase2.body}
    {fields.phrase2.direction === 'right' ? fields.word2 && fields.word2.word : fields.phrase2 && fields.phrase2.body}
  </h1>
  <div class="form-field">
    <label for="phrase1">Phrase:</label>
    <select id="phrase1" bind:value={fields.phrase1}>
      <option value="">Select Phrase</option>
      {#each phrases as phrase}
        <option id={phrases._id} value={phrase}>{phrase.body}</option>
      {/each}
    </select>
    <div class="error">{errors.phrase1}</div>
    <label for="word1">Word:</label>
    <select id="word1" bind:value={fields.word1}>
      <option value="">Select Word</option>
      {#each words as word}
        <option id={word._id} value={word}>{word.word}</option>
      {/each}
    </select>
    <div class="error">{errors.word1}</div>
    
    <select id="middleWord" bind:value={fields.middleWord}>
      <option value="">Select Word</option>
      {#each middleWords as mid}
        <option value={mid}>{mid}</option>
      {/each}
    </select>

    <label for="phrase2">Phrase:</label>
    <select id="phrase2" bind:value={fields.phrase2}>
      <option value="">Select Phrase</option>
      {#each phrases as phrase}
        <option id={phrases._id} value={phrase}>{phrase.body}</option>
      {/each}
    </select>
    <div class="error">{errors.phrase2}</div>
    <label for="word1">Word:</label>
    <select id="word1" bind:value={fields.word2}>
      <option value="">Select Word</option>
      {#each words as word}
        <option id={word._id} value={word}>{word.word}</option>
      {/each}
    </select>
    <div class="error">{errors.word2}</div>
  </div>
  <!-- <Button type="secondary" flat={true}>Add Phrase</Button> -->
</form>
