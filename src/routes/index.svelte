<script context="module">
	import { onMount } from 'svelte';
	import PrismicDOM from 'prismic-dom';
	import { Client } from '../../prismic-config.js';

	let homepage = null;
	
	export async function preload({ params, query }) {
		homepage = await Client.getSingle('homepage');

		if (homepage) {
			return { homepage };
		} else {
			this.error(res.status, data.message);
		}
	}
</script>

<script>
	export let homepage;
</script>

<style>
	h1, figure, :global(.home p) {
		text-align: center;
		margin: 0 auto;
	}

	h1 {
		font-size: 2.8em;
		text-transform: uppercase;
		font-weight: 700;
		margin: 0 0 0.5em 0;
	}

	figure {
		margin: 0 0 1em 0;
	}

	img {
		width: 100%;
		max-width: 400px;
		margin: 0 0 1em 0;
	}

	:global(.home p) {
		margin: 1em auto;
	}

	@media (min-width: 480px) {
		h1 {
			font-size: 4em;
		}
	}
</style>

<svelte:head>
	<title>Sapper project template</title>
</svelte:head>

<div class="home">
	<h1>{PrismicDOM.RichText.asText(homepage.data.title)}</h1>

	<figure>
		<img alt={homepage.data.main_image.alt} src={homepage.data.main_image.url}>
		<figcaption>{PrismicDOM.RichText.asText(homepage.data.image_caption)}</figcaption>
	</figure>

	{@html PrismicDOM.RichText.asHtml(homepage.data.content)}
</div>
