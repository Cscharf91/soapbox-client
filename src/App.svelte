<script>
	import Tabs from './shared/Tabs.svelte';
	import axios from 'axios';
	import { onMount } from 'svelte';
import Categories from './components/Categories.svelte';
import Words from './components/Words.svelte';
import CreateCategoryForm from './components/CreateCategoryForm.svelte';
import CreateWordForm from './components/CreateWordForm.svelte';

	let items = ['Categories', 'Words'];
	let activeItem = 'Categories';
	
	let API = 'http://localhost:5000/api';

	let words = [];
	let categories = [];

	onMount(async () => {
		const catRes = await fetch(`${API}/categories`);
		const wordRes = await fetch(`${API}/words`);
		categories = await catRes.json();
		words = await wordRes.json();
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

	const handleDeleteWord = (e) => {
		const deletedId = e.detail;
		words = words.filter(word => word._id !== deletedId);
	}
</script>

<main>
	<h1>Word/Category Creator</h1>
	<Tabs {activeItem} {items} on:tabChange={handleTabChange} />
	{#if activeItem === 'Categories'}
		<CreateCategoryForm on:add={handleAddCat} />
		<Categories {API} {categories} on:DELETE={handleDeleteCat} />
	{:else if activeItem === 'Words'}
		<CreateWordForm {categories} on:add={handleAddWord} />
		<Words {API} {words} {categories} on:DELETE={handleDeleteWord} />
	{/if}
</main>

<style>

	h1 {
		color: #003a86;
		text-transform: uppercase;
		text-align: center;
		font-size: 4em;
		font-weight: 100;
	}
</style>