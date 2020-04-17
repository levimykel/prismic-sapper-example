<script context="module">
	import { onMount } from 'svelte';
	import PrismicDOM from 'prismic-dom';
	import { Client, linkResolver } from '../../../prismic-config.js';

	let document = null;
	
	export async function preload({ params, query }) {
		const uid = params.uid;
		document = await Client.getByUID('blog_post', uid);

		if (document) {
			const title = PrismicDOM.RichText.asText(document.data.title);
			return { document, title };
		} else {
			this.error(res.status, data.message);
		}
	}
</script>

<script>
	export let document;
	export let title;
</script>

<style>
	.content :global(h2) {
		font-size: 1.4em;
		font-weight: 500;
	}

	.content :global(pre) {
		background-color: #f9f9f9;
		box-shadow: inset 1px 1px 5px rgba(0,0,0,0.05);
		padding: 0.5em;
		border-radius: 2px;
		overflow-x: auto;
	}

	.content :global(pre) :global(code) {
		background-color: transparent;
		padding: 0;
	}

	.content :global(ul) {
		line-height: 1.5;
	}

	.content :global(li) {
		margin: 0 0 0.5em 0;
	}
</style>

<svelte:head>
	<title>{title}</title>
</svelte:head>

<h1>{title}</h1>
<div class='content'>
	{@html PrismicDOM.RichText.asHtml(document.data.content, linkResolver)}
</div>
