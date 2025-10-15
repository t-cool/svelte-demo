<script lang="ts">
	import { browser } from '$app/environment';
	import { onMount } from 'svelte';

	let mounted = false;

	onMount(() => {
		mounted = true;
	});
</script>

<svelte:head>
	<title>アバウト - SvelteKit Demo</title>
</svelte:head>

<div class="text-column">
	<h1>このアプリについて</h1>

	<p>
		このSvelteKitデモアプリは、モダンなWebアプリケーション開発のベストプラクティスを
		示すために作成されました。
	</p>

	<p>使用技術:</p>
	<ul>
		<li><strong>SvelteKit</strong> - フルスタックWebフレームワーク</li>
		<li><strong>TypeScript</strong> - 型安全なJavaScript</li>
		<li><strong>Vite</strong> - 高速ビルドツール</li>
		<li><strong>CSS3</strong> - モダンスタイリング</li>
	</ul>

	<p>
		SvelteKitの特徴:
	</p>
	<ul>
		<li>🚀 ゼロ設定でSSR/SPA/SSGに対応</li>
		<li>⚡ 高速な開発サーバーとホットリロード</li>
		<li>📦 バンドルサイズの最適化</li>
		<li>🎯 TypeScript完全サポート</li>
		<li>🛠️ ファイルベースルーティング</li>
	</ul>

	{#if mounted}
		<p class="info">
			このページは
			{#if browser}
				ブラウザ
			{:else}
				サーバー
			{/if}
			でレンダリングされました。
		</p>
	{/if}
</div>

<style>
	.text-column {
		max-width: 64rem;
		margin: 0 auto;
		padding: 2rem 1rem;
	}

	h1 {
		font-size: 2.5rem;
		margin-bottom: 2rem;
		color: var(--color-theme-1);
	}

	ul {
		line-height: 1.8;
	}

	li {
		margin-bottom: 0.5rem;
	}

	.info {
		margin-top: 2rem;
		padding: 1rem;
		background: rgba(255, 62, 0, 0.1);
		border-left: 4px solid var(--color-theme-1);
		border-radius: 4px;
	}
</style>