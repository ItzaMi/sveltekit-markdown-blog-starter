<script context="module">
	const posts = import.meta.glob('./*.svx');

	let body = [];

	for (const path in posts) {
		body.push(posts[path]().then(({ metadata }) => metadata));
	}

	/**
	 * @type {import('@sveltejs/kit').Load}
	 */
	export async function load({ url, params, fetch }) {
		const posts = await Promise.all(body);

		return {
			props: {
				posts
			}
		};
	}
</script>

<script>
	export let posts;
</script>

<ul>
	<h1>Blog</h1>
	{#each posts as { title, outline, slug }}
		<li>
			<a rel="prefetch" href="blog/{slug}">
				<h2>
					{title}
				</h2>

				<p>
					{outline}
				</p>
			</a>
		</li>
	{/each}
</ul>
