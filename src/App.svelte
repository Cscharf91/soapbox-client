<script>
	import Tabs from './shared/Tabs.svelte';
	import { onMount } from 'svelte';
	import Categories from './components/Categories.svelte';
	import Words from './components/Words.svelte';
	import CreateCategoryForm from './components/CreateCategoryForm.svelte';
	import CreateWordForm from './components/CreateWordForm.svelte';
	import Soapstone from './components/Soapstone.svelte';
	import CreatePhraseForm from './components/CreatePhraseForm.svelte';
	import Phrases from './components/Phrases.svelte';
	import CreateSoapstoneForm from './components/CreateSoapstoneForm.svelte';

	let items = ['Soapstone', 'Categories', 'Words', 'Phrases'];
	let activeItem = 'Soapstone';
	
	let API = 'https://stormy-fortress-52595.herokuapp.com/api';
	// let API = 'http://localhost:5000/api';

	let words = [];
	let categories = [];
	let phrases = [];

	onMount(async () => {
		const phraseRes = await fetch(`${API}/phrases`);
		const catRes = await fetch(`${API}/categories`);
		const wordRes = await fetch(`${API}/words`);
		phrases = await phraseRes.json();
		categories = await catRes.json();
		words = await wordRes.json();
		console.log(words);
	})

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
		categories = [data, ...categories];
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
		phrases = [data, ...phrases];
		console.log(phrases);
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
		console.log(data.word);
		words = [data.word, ...words];
		// console.log(data, words);
	}

	const handleDeleteCat = (e) => {
		const deletedId = e.detail;
		categories = categories.filter(cat => cat._id !== deletedId);
	}

	const handleDeletePhrase = (e) => {
		const deletedId = e.detail;
		phrases = phrases.filter(phrase => phrase._id !== deletedId);
	}

	const handleDeleteWord = (e) => {
		const deletedId = e.detail;
		words = words.filter(word => word._id !== deletedId);
	}
</script>

<main>
	<h1>Soapstone</h1>
	<Tabs {activeItem} {items} on:tabChange={handleTabChange} />
	{#if activeItem === 'Categories'}
		<CreateCategoryForm on:add={handleAddCat} />
		<Categories {API} {categories} on:DELETE={handleDeleteCat} />
	{:else if activeItem === 'Words'}
		<CreateWordForm {categories} on:add={handleAddWord} />
		<Words {API} {words} {categories} on:DELETE={handleDeleteWord} />
	{:else if activeItem === 'Soapstone'}
		<CreateSoapstoneForm {words} {categories} {phrases} />
	{:else}
		<CreatePhraseForm on:add={handleAddPhrase} />
		<Phrases {API} {phrases} on:DELETE={handleDeletePhrase} />
	{/if}
</main>

<style>

	h1 {
		color: #003a86;
		text-transform: uppercase;
		text-align: center;
		font-size: 4em;
	}

	@media(max-width: 500px) {
		h1 {
			font-size: 2.5em;
		}
	}
</style>