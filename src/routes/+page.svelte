<script>
	import Header from '../components/Header.svelte';
	import Button from '../components/Button.svelte';
  import Carousel from '../components/Carousel.svelte';
  import Background from '../components/Background.svelte';
  import Canvas from '../components/Canvas.svelte';
</script>

<div id=main>
	<Canvas />
	<Carousel />

	<div class='bot'>
		<div class="top">
			<div class='title'>
				<h1>theobbb</h1>
				<div class="lang">
					<Button>fr</Button>
				</div>
			</div>
			<div class="contact">
				<Button>Contact</Button>
			</div>
		</div>
		<div class="text">
			<p>
				Je collabore avec d’autres créatifs pour vous offrir des solutions performantes et élégantes. 
			</p>
			<p>
				Vitrine en ligne, plateforme de commerce, application interactive... je suis prêt à relever le défi.
			</p>
			<p>
				Expliquez-moi votre idée, je vous reviendrez sous peu. Au plaisir de travailler avec vous!
			</p>
		</div>
	</div>
</div>

<style>

	.bot {
		position: fixed;
		width: 100%;
		bottom: 0;
		left: 0;
		padding-bottom: 24px;
	}
	.bot .top {
		display: flex;
		justify-content: space-between;
		align-items: flex-end;
	}

	.title {
		display: flex;
		align-items: flex-end;

		
	}
	h1 {
		font-size: 8em;
		opacity: 0.8;
		line-height: 1;
		margin: 0;
	}
	.text { 
		display: flex;
		justify-content: flex-end;
		font-size: 1.2rem;
		
	}
	.text p {
		margin: 0 24px;
		max-width: 500px;
	}
</style>