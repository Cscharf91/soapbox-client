<script>
  import { createEventDispatcher } from "svelte";
  import Button from "../shared/Button.svelte";

  let dispatch = createEventDispatcher();
  let fields = { name: "" };
  let errors = { name: "" };
  let valid = false;

  const handleSubmit = () => {
    valid = true;

    if (fields.name.trim().length < 1) {
      valid = false;
      errors.name = "Must have a name.";
    } else {
      errors.name = "";
    }

    if (valid) {
      let newCategory = { ...fields };
      fields = { name: "" };
      dispatch("add", newCategory);
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

  @media(max-width: 500px) {
    input {
      height: 30px;
      width: 60%;
    }
  }
</style>

<form on:submit|preventDefault={handleSubmit}>
  <div class="form-field">
    <label for="name">Category Name:</label>
    <input type="text" id="name" bind:value={fields.name} />
    <div class="error">{errors.name}</div>
  </div>
  <Button type="secondary" flat={true}>Add Category</Button>
</form>
