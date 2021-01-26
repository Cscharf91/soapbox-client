<script>
	import { Router, Link, Route } from 'svelte-routing';
	import Create from './components/Create.svelte';

  export let url = "";
</script>

<Router url="{url}">
	<div class="hero">
		<Link to="/"><h1>Soapstone</h1></Link>
		<div class="links">
			<Link to="/create"><p>Teacher Portal</p></Link>
		</div>
	</div>
	<Route path="/create" component="{Create}" />

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