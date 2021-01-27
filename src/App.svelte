<script>
import { onMount } from 'svelte';

	import { Router, Link, Route } from 'svelte-routing';
	import Create from './components/Create.svelte';
	import Prompt from './components/Prompt.svelte';
	import Prompts from './components/Prompts.svelte';
  import { words, phrases, categories, prompts } from './Stores.js';
	
	import axios from 'axios';

	export let url = "";
  let API = 'https://stormy-fortress-52595.herokuapp.com/api';
	
	onMount(async () => {
		await getWords();
		await getCategories();
		await getPrompts();
		await getPhrases();
	})

	const getWords = async () => {
		try {
			const { data } = await axios.get(`${API}/words`);
			$words = data;
		} catch (err) {
			console.log(err);
		}
	}

	const getCategories = async () => {
		try {
			const { data } = await axios.get(`${API}/categories`);
			$categories = data;
		} catch (err) {
			console.log(err);
		}
	}

	const getPrompts = async () => {
		try {
			const { data } = await axios.get(`${API}/prompts`);
			$prompts = data;
		} catch (err) {
			console.log(err);
		}
	}

	const getPhrases = async () => {
		try {
			const { data } = await axios.get(`${API}/phrases`);
			$phrases = data;
		} catch (err) {
			console.log(err);
		}
	}
</script>

<Router url="{url}">
	<div class="hero">
		<Link to="/"><h1>Soapstone</h1></Link>
		<div class="links">
			<Link to="/create"><p>Teacher Portal</p></Link>
		</div>
	</div>
	<Route path="/" component="{Prompts}" />
	<Route path="/create" component="{Create}" />
	<Route path="/prompts/:id" let:params>
		<Prompt id={params.id} />
	</Route>

</Router>

<style>
	.hero {
		width: 100%;
		padding-top: 75px;
		padding-bottom: 20px;
		background: rgb(248, 248, 248);
	}

	h1 {
		color: #003a86;
		text-transform: uppercase;
		text-align: center;
		font-size: 4em;
	}

	.links {
		text-align: center;
	}

	:global(a) {
		text-decoration: none;
	}

	@media(max-width: 500px) {
		h1 {
			font-size: 2.5em;
		}
	}
</style>