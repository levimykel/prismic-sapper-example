<script context="module">
	import { onMount } from 'svelte';
	import PrismicDOM from 'prismic-dom';
	import { Client } from '../../prismic-config.js';

	let document = null;
	
	export async function preload({ params, query }) {
		document = await Client.getSingle('about');

		if (document) {
			return { document };
		} else {
			this.error(res.status, data.message);
		}
	}
</script>

<script>
	export let document;
</script>

<svelte:head>
	<title>About</title>
</svelte:head>

<style>
	p {
		text-align: left;
	}
</style>

<div class="about">
	<h1>{PrismicDOM.RichText.asText(document.data.title)}</h1>
	{@html PrismicDOM.RichText.asHtml(document.data.content)}
</div>
