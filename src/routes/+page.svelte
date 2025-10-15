<script lang="ts">
	import { onMount } from 'svelte';

	let message = '';
	let loading = true;

	onMount(() => {
		setTimeout(() => {
			message = 'SvelteKitへようこそ！';
			loading = false;
		}, 1000);
	});
</script>

<svelte:head>
	<title>ホーム - SvelteKit Demo</title>
	<meta name="description" content="SvelteKitのデモアプリケーション" />
</svelte:head>

<section class="hero">
	<h2>SvelteKit Demo App</h2>

	{#if loading}
		<div class="loading">
			<div class="spinner"></div>
			<p>読み込み中...</p>
		</div>
	{:else}
		<p class="welcome-message">{message}</p>
	{/if}

	<div class="features">
		<div class="feature">
			<h3>⚡ 高速</h3>
			<p>Viteベースの開発環境で高速なビルドとホットリロード</p>
		</div>
		<div class="feature">
			<h3>🎯 型安全</h3>
			<p>TypeScriptによる型安全な開発体験</p>
		</div>
		<div class="feature">
			<h3>📱 レスポンシブ</h3>
			<p>モバイルファーストなレスポンシブデザイン</p>
		</div>
	</div>
</section>

<style>
	.hero {
		text-align: center;
		padding: 2rem 0;
	}

	.hero h2 {
		font-size: 2.5rem;
		margin-bottom: 1rem;
		color: #ff3e00;
	}

	.loading {
		margin: 2rem 0;
	}

	.spinner {
		width: 40px;
		height: 40px;
		margin: 0 auto 1rem;
		border: 4px solid #f3f3f3;
		border-top: 4px solid #ff3e00;
		border-radius: 50%;
		animation: spin 1s linear infinite;
	}

	@keyframes spin {
		0% { transform: rotate(0deg); }
		100% { transform: rotate(360deg); }
	}

	.welcome-message {
		font-size: 1.5rem;
		margin: 2rem 0;
		color: #333;
	}

	.features {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
		gap: 2rem;
		margin: 3rem 0;
	}

	.feature {
		padding: 2rem;
		border: 1px solid #e0e0e0;
		border-radius: 8px;
		background: #f9f9f9;
	}

	.feature h3 {
		margin-bottom: 1rem;
		color: #ff3e00;
	}
</style>