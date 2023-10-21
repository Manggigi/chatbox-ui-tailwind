<script>
	import '../app.css';
</script>

<div class="app">
	<!-- <Header /> -->

	<main>
		<slot />
	</main>

	<footer>
		<p class="text-sm text-neutral-500 md:text-xl font-light">
			Made by
			<a
				href={'https://www.linkedin.com/in/geianmarkdenorte'}
				target="_blank"
				class="cursor-pointer"
			>
				@Manggigi
			</a>
		</p>
	</footer>
</div>

<style>
	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	footer a {
		font-weight: bold;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
