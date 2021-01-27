<script>
  import { createEventDispatcher } from "svelte";
  import Button from "../shared/Button.svelte";

  let dispatch = createEventDispatcher();
  let fields = { body: "" };
  let errors = { body: "" };
  let valid = false;

  const handleSubmit = () => {
    valid = true;

    if (fields.body.length < 1) {
      valid = false;
      errors.body = "Must have a body.";
    } else {
      errors.body = "";
    }

    if (valid) {
      let newPrompt = { ...fields };
      console.log(newPrompt);
      fields = { body: "" };
      dispatch("add", newPrompt);
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
    margin: 20px auto;
    text-align: left;
  }

  .error {
    font-weight: bold;
    font-size: 12px;
    color: #d91b42;
  }

	@media(max-width: 500px) {
    input {
      height: 30px;
      width: 60%;
    }
  }
</style>

<form on:submit|preventDefault={handleSubmit}>
  <div class="form-field">
    <label for="body">Body:</label>
    <input type="text" id="body" bind:value={fields.body} />
  </div>
  <Button type="secondary" flat={true}>Add Prompt</Button>
</form>
