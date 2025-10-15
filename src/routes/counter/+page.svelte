<script lang="ts">
	import { spring } from 'svelte/motion';
	import { fade, scale } from 'svelte/transition';

	let count = 0;
	let displayed_count = spring();
	$: displayed_count.set(count);
	$: offset = modulo($displayed_count, 1);

	function modulo(n: number, m: number) {
		return ((n % m) + m) % m;
	}

	function increment() {
		count += 1;
	}

	function decrement() {
		count -= 1;
	}

	function reset() {
		count = 0;
	}
</script>

<svelte:head>
	<title>カウンター - SvelteKit Demo</title>
</svelte:head>

<div class="counter">
	<h1>インタラクティブカウンター</h1>

	<div class="counter-display">
		<div class="counter-container">
			<div
				class="counter-viewport"
				style="transform: translate(0, {100 * offset}%)"
			>
				<div class="counter-digit" aria-hidden="true">
					{Math.floor($displayed_count + 1)}
				</div>
				<div class="counter-digit">
					{Math.floor($displayed_count)}
				</div>
			</div>
		</div>
	</div>

	<div class="controls">
		<button
			on:click={decrement}
			disabled={count <= 0}
			class="btn btn-secondary"
		>
			-
		</button>

		<button on:click={reset} class="btn btn-neutral">
			リセット
		</button>

		<button on:click={increment} class="btn btn-primary">
			+
		</button>
	</div>

	{#if count > 0}
		<div class="message" in:fade={{ duration: 300 }} out:scale={{ duration: 200 }}>
			<p>カウント: {count} 回クリックしました！</p>
			{#if count >= 10}
				<p class="achievement">🎉 10回達成！素晴らしい！</p>
			{/if}
		</div>
	{/if}

	<div class="description">
		<h3>機能説明</h3>
		<ul>
			<li>スプリングアニメーション付きカウンター表示</li>
			<li>ボタンの状態管理（減算ボタンは0以下で無効化）</li>
			<li>フェード・スケールトランジション</li>
			<li>到達アチーブメント表示</li>
		</ul>
	</div>
</div>

<style>
	.counter {
		text-align: center;
		padding: 2rem;
		max-width: 32rem;
		margin: 0 auto;
	}

	h1 {
		color: var(--color-theme-1);
		margin-bottom: 2rem;
	}

	.counter-display {
		margin: 2rem 0;
	}

	.counter-container {
		display: flex;
		justify-content: center;
		margin: 2rem 0;
	}

	.counter-viewport {
		width: 8rem;
		height: 4rem;
		overflow: hidden;
		display: flex;
		flex-direction: column-reverse;
		position: relative;
		background: rgba(255, 255, 255, 0.1);
		border-radius: 8px;
		border: 2px solid var(--color-theme-1);
	}

	.counter-digit {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
		font-size: 2.5rem;
		font-weight: bold;
		color: var(--color-theme-1);
	}

	.controls {
		display: flex;
		gap: 1rem;
		justify-content: center;
		margin: 2rem 0;
		flex-wrap: wrap;
	}

	.btn {
		font-size: 1.2rem;
		padding: 0.8rem 1.5rem;
		border: none;
		border-radius: 8px;
		cursor: pointer;
		font-weight: bold;
		transition: all 0.2s;
		min-width: 3rem;
	}

	.btn:hover:not(:disabled) {
		transform: translateY(-2px);
		box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
	}

	.btn:disabled {
		opacity: 0.5;
		cursor: not-allowed;
	}

	.btn-primary {
		background: var(--color-theme-1);
		color: white;
	}

	.btn-secondary {
		background: var(--color-theme-2);
		color: white;
	}

	.btn-neutral {
		background: #666;
		color: white;
	}

	.message {
		margin: 2rem 0;
		padding: 1rem;
		background: rgba(255, 62, 0, 0.1);
		border-radius: 8px;
		border: 1px solid rgba(255, 62, 0, 0.3);
	}

	.achievement {
		font-size: 1.2rem;
		font-weight: bold;
		color: var(--color-theme-1);
		margin-top: 0.5rem;
	}

	.description {
		margin-top: 3rem;
		text-align: left;
		background: rgba(255, 255, 255, 0.1);
		padding: 1.5rem;
		border-radius: 8px;
	}

	.description h3 {
		color: var(--color-theme-1);
		margin-bottom: 1rem;
	}

	.description ul {
		list-style-type: none;
		padding: 0;
	}

	.description li {
		padding: 0.3rem 0;
		padding-left: 1.5rem;
		position: relative;
	}

	.description li::before {
		content: '✓';
		position: absolute;
		left: 0;
		color: var(--color-theme-1);
		font-weight: bold;
	}
</style>