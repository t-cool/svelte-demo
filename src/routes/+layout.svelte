<script lang="ts">
	import './styles.css';
</script>

<div class="app">
	<header>
		<h1>🎯 SvelteKit Demo</h1>
		<nav>
			<ul>
				<li><a href="/">ホーム</a></li>
				<li><a href="/about">アバウト</a></li>
				<li><a href="/counter">カウンター</a></li>
				<li><a href="/todos">TODO</a></li>
			</ul>
		</nav>
	</header>

	<main>
		<slot />
	</main>

	<footer>
		<p>&copy; 2025 SvelteKit Demo App</p>
	</footer>
</div>