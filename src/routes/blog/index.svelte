<script context="module">
	import { onMount } from 'svelte';
	import Prismic from 'prismic-javascript';
	import PrismicDOM from 'prismic-dom';
	import { Client, linkResolver } from '../../../prismic-config.js';

	let document = null;
	let posts = null;
	
	export async function preload({ params, query }) {
		document = await Client.getSingle('blog_home');
		const postResponse = await Client.query(
			Prismic.Predicates.at('document.type', 'blog_post'),
			{ orderings: '[my.blog_post.publication_date desc]' }
		)
		posts = postResponse.results

		if (document && posts) {
			return { document, posts };
		} else {
			this.error(res.status, data.message);
		}
	}
</script>

<script>
	export let document;
	export let posts;
</script>

<style>
	ul {
		margin: 0 0 1em 0;
		line-height: 1.5;
	}
</style>

<svelte:head>
	<title>Blog</title>
</svelte:head>

<div class="blog-home">
	<h1>{PrismicDOM.RichText.asText(document.data.title)}</h1>

	<ul>
		{#each posts as post}
			<!-- we're using the non-standard `rel=prefetch` attribute to
					tell Sapper to load the data for the page as soon as
					the user hovers over the link or taps it, instead of
					waiting for the 'click' event -->
			<li>
				<a rel='prefetch' href={linkResolver(post)}>
					{PrismicDOM.RichText.asText(post.data.title)}
				</a>
			</li>
		{/each}
	</ul>
</div>

