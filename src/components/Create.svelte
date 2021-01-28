<script>
  import Tabs from '../shared/Tabs.svelte';
	import { onMount } from 'svelte';
	import Categories from './Categories.svelte';
	import Words from './Words.svelte';
	import CreateCategoryForm from './CreateCategoryForm.svelte';
	import CreateWordForm from './CreateWordForm.svelte';
	// import Soapstone from './Soapstone.svelte';
	import CreatePhraseForm from './CreatePhraseForm.svelte';
	import Phrases from './Phrases.svelte';
	import CreateSoapstoneForm from './CreateSoapstoneForm.svelte';
  import CreatePrompt from './CreatePrompt.svelte';

  let items = ['Prompts', 'Categories', 'Words', 'Phrases'];
  let activeItem = 'Prompts';

  let API = 'https://stormy-fortress-52595.herokuapp.com/api';
  // let API = 'http://localhost:5000/api';

  import { words, phrases, categories, prompts } from '../Stores.js';
import Prompts from './Prompts.svelte';

  const handleTabChange = (e) => {
    activeItem = e.detail;
  }

  const handleAddCat = async (e) => {
    const newCat = e.detail;
    const res = await fetch(`${API}/categories`, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(newCat)
    });
    const data = await res.json();
    $categories = [data, ...$categories];
  }

  const handleAddPhrase = async (e) => {
    const newPhrase = e.detail;
    const res = await fetch(`${API}/phrases`, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(newPhrase)
    });
    const data = await res.json();
    $phrases = [data, ...$phrases];
  }

  const handleAddPrompt = async (e) => {
    const newPrompt = e.detail;
    const res = await fetch(`${API}/prompts`, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(newPrompt)
    });
    const data = await res.json();
    $prompts = [data, ...$prompts];
  }

  const handleAddWord = async (e) => {
    const newWord = e.detail;
    const res = await fetch(`${API}/words`, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(newWord)
    });
    const data = await res.json();
    data.word.category = data.category;
    $words = [data.word, ...$words];
  }

  const handleDeleteCat = (e) => {
    const deletedId = e.detail;
    $categories = $categories.filter(cat => cat._id !== deletedId);
  }
  
  const handleDeletePhrase = (e) => {
    const deletedId = e.detail;
    $phrases = phrases.filter(phrase => phrase._id !== deletedId);
  }
  
  const handleDeleteWord = (e) => {
    const deletedId = e.detail;
    $words = words.filter(word => word._id !== deletedId);
  }
</script>


<Tabs {activeItem} {items} on:tabChange={handleTabChange} />
{#if activeItem === 'Categories'}
  <CreateCategoryForm on:add={handleAddCat} />
  <Categories {API} on:DELETE={handleDeleteCat} />
{:else if activeItem === 'Words'}
  <CreateWordForm on:add={handleAddWord} />
  <Words {API} on:DELETE={handleDeleteWord} />
{:else if activeItem === 'Phrases'}
  <CreatePhraseForm on:add={handleAddPhrase} />
  <Phrases {API} on:DELETE={handleDeletePhrase} />
{:else}
  <CreatePrompt on:add={handleAddPrompt} />
  <Prompts teacher={true} />
{/if}