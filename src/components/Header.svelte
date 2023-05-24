<script>
	import Test from './Test.svelte'

	let links = [
		{ name: 'Home', url: '/' },
		{ name: 'About', url: '/about' },
		{ name: 'Contact', url: '/contact' },
		{ name: 'ff', url: '/ff' }
	];

	let open = false

	console.log('test', open)

</script>

<header>
	
	<h2>Weed Shop</h2>

	<button on:click={() => open = !open}>
		{open ? 'Close' : 'Open'}
	</button>

	<div class="links">
	{#each links as link}
		
		<h2>
			<a href={link.url}>
			{link.name}
			</a>
		</h2>
	{/each}
	</div>
</header>

<style>
	header {
		padding: 24px 48px;
		display: flex;
		justify-content: space-between;
	}

	.links {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}

	.links h2 a {
		text-decoration: none;
		color: inherit;
		
	}

	.links h2 {
		margin: 0 24px;
		transition: .4s ease;
	}
	.links h2:hover {
		opacity: 0.6;
		transform: scale(1.1);
		
	}

</style>