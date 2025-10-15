<script lang="ts">
	import { flip } from 'svelte/animate';
	import { fade, slide } from 'svelte/transition';

	interface Todo {
		id: number;
		text: string;
		completed: boolean;
		createdAt: Date;
	}

	let todos: Todo[] = [
		{ id: 1, text: 'SvelteKitを学ぶ', completed: true, createdAt: new Date() },
		{ id: 2, text: 'デモアプリを作る', completed: false, createdAt: new Date() },
		{ id: 3, text: 'TypeScriptを活用する', completed: false, createdAt: new Date() }
	];

	let newTodoText = '';
	let filter: 'all' | 'active' | 'completed' = 'all';

	$: filteredTodos = todos.filter(todo => {
		if (filter === 'active') return !todo.completed;
		if (filter === 'completed') return todo.completed;
		return true;
	});

	$: completedCount = todos.filter(todo => todo.completed).length;
	$: activeCount = todos.length - completedCount;

	function addTodo() {
		if (newTodoText.trim()) {
			todos = [...todos, {
				id: Date.now(),
				text: newTodoText.trim(),
				completed: false,
				createdAt: new Date()
			}];
			newTodoText = '';
		}
	}

	function toggleTodo(id: number) {
		todos = todos.map(todo =>
			todo.id === id ? { ...todo, completed: !todo.completed } : todo
		);
	}

	function deleteTodo(id: number) {
		todos = todos.filter(todo => todo.id !== id);
	}

	function clearCompleted() {
		todos = todos.filter(todo => !todo.completed);
	}

	function handleKeydown(event: KeyboardEvent) {
		if (event.key === 'Enter') {
			addTodo();
		}
	}
</script>

<svelte:head>
	<title>TODO - SvelteKit Demo</title>
</svelte:head>

<div class="todos">
	<h1>TODOリスト</h1>

	<div class="add-todo">
		<input
			bind:value={newTodoText}
			on:keydown={handleKeydown}
			placeholder="新しいTODOを入力..."
			class="todo-input"
		/>
		<button on:click={addTodo} class="add-btn" disabled={!newTodoText.trim()}>
			追加
		</button>
	</div>

	<div class="filters">
		<button
			class="filter-btn {filter === 'all' ? 'active' : ''}"
			on:click={() => filter = 'all'}
		>
			すべて ({todos.length})
		</button>
		<button
			class="filter-btn {filter === 'active' ? 'active' : ''}"
			on:click={() => filter = 'active'}
		>
			未完了 ({activeCount})
		</button>
		<button
			class="filter-btn {filter === 'completed' ? 'active' : ''}"
			on:click={() => filter = 'completed'}
		>
			完了済み ({completedCount})
		</button>
	</div>

	{#if filteredTodos.length === 0}
		<div class="empty-state" in:fade>
			{#if filter === 'all'}
				<p>📝 TODOがありません。新しく追加してみましょう！</p>
			{:else if filter === 'active'}
				<p>✨ すべてのTODOが完了しています！</p>
			{:else}
				<p>📋 完了したTODOがありません</p>
			{/if}
		</div>
	{:else}
		<ul class="todo-list">
			{#each filteredTodos as todo (todo.id)}
				<li
					class="todo-item {todo.completed ? 'completed' : ''}"
					animate:flip={{ duration: 300 }}
					in:slide={{ duration: 300 }}
					out:slide={{ duration: 200 }}
				>
					<div class="todo-content">
						<label class="checkbox-container">
							<input
								type="checkbox"
								checked={todo.completed}
								on:change={() => toggleTodo(todo.id)}
							/>
							<span class="checkmark"></span>
						</label>
						<span class="todo-text">{todo.text}</span>
						<span class="todo-date">
							{todo.createdAt.toLocaleDateString('ja-JP')}
						</span>
					</div>
					<button
						class="delete-btn"
						on:click={() => deleteTodo(todo.id)}
						title="削除"
					>
						×
					</button>
				</li>
			{/each}
		</ul>
	{/if}

	{#if completedCount > 0}
		<div class="actions" transition:fade>
			<button on:click={clearCompleted} class="clear-btn">
				完了済みをクリア ({completedCount})
			</button>
		</div>
	{/if}

	<div class="stats">
		<div class="stat">
			<span class="stat-number">{todos.length}</span>
			<span class="stat-label">総数</span>
		</div>
		<div class="stat">
			<span class="stat-number">{activeCount}</span>
			<span class="stat-label">未完了</span>
		</div>
		<div class="stat">
			<span class="stat-number">{completedCount}</span>
			<span class="stat-label">完了</span>
		</div>
	</div>
</div>

<style>
	.todos {
		max-width: 48rem;
		margin: 0 auto;
		padding: 2rem;
	}

	h1 {
		text-align: center;
		color: var(--color-theme-1);
		margin-bottom: 2rem;
	}

	.add-todo {
		display: flex;
		gap: 1rem;
		margin-bottom: 2rem;
	}

	.todo-input {
		flex: 1;
		padding: 1rem;
		border: 2px solid #e0e0e0;
		border-radius: 8px;
		font-size: 1rem;
		transition: border-color 0.2s;
	}

	.todo-input:focus {
		outline: none;
		border-color: var(--color-theme-1);
	}

	.add-btn {
		padding: 1rem 2rem;
		background: var(--color-theme-1);
		color: white;
		border: none;
		border-radius: 8px;
		cursor: pointer;
		font-weight: bold;
		transition: all 0.2s;
	}

	.add-btn:hover:not(:disabled) {
		background: #e63900;
		transform: translateY(-1px);
	}

	.add-btn:disabled {
		opacity: 0.5;
		cursor: not-allowed;
	}

	.filters {
		display: flex;
		gap: 0.5rem;
		margin-bottom: 2rem;
		flex-wrap: wrap;
	}

	.filter-btn {
		padding: 0.5rem 1rem;
		border: 2px solid var(--color-theme-1);
		background: transparent;
		color: var(--color-theme-1);
		border-radius: 20px;
		cursor: pointer;
		transition: all 0.2s;
		font-size: 0.9rem;
	}

	.filter-btn:hover {
		background: rgba(255, 62, 0, 0.1);
	}

	.filter-btn.active {
		background: var(--color-theme-1);
		color: white;
	}

	.todo-list {
		list-style: none;
		padding: 0;
		margin: 0 0 2rem 0;
	}

	.todo-item {
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 1rem;
		margin-bottom: 0.5rem;
		background: rgba(255, 255, 255, 0.1);
		border-radius: 8px;
		border: 1px solid rgba(255, 255, 255, 0.2);
		transition: all 0.2s;
	}

	.todo-item:hover {
		background: rgba(255, 255, 255, 0.2);
	}

	.todo-item.completed {
		opacity: 0.7;
		background: rgba(0, 200, 0, 0.1);
	}

	.todo-content {
		display: flex;
		align-items: center;
		gap: 1rem;
		flex: 1;
	}

	.checkbox-container {
		position: relative;
		cursor: pointer;
		font-size: 1.2rem;
	}

	.checkbox-container input {
		position: absolute;
		opacity: 0;
		cursor: pointer;
		height: 0;
		width: 0;
	}

	.checkmark {
		position: relative;
		display: inline-block;
		height: 20px;
		width: 20px;
		background-color: transparent;
		border: 2px solid var(--color-theme-1);
		border-radius: 4px;
		transition: all 0.2s;
	}

	.checkbox-container input:checked ~ .checkmark {
		background-color: var(--color-theme-1);
	}

	.checkmark:after {
		content: "";
		position: absolute;
		display: none;
		left: 6px;
		top: 2px;
		width: 5px;
		height: 10px;
		border: solid white;
		border-width: 0 3px 3px 0;
		transform: rotate(45deg);
	}

	.checkbox-container input:checked ~ .checkmark:after {
		display: block;
	}

	.todo-text {
		flex: 1;
		font-size: 1.1rem;
	}

	.todo-item.completed .todo-text {
		text-decoration: line-through;
		color: rgba(0, 0, 0, 0.5);
	}

	.todo-date {
		font-size: 0.8rem;
		color: rgba(0, 0, 0, 0.6);
		margin-left: auto;
		margin-right: 1rem;
	}

	.delete-btn {
		background: #ff4757;
		color: white;
		border: none;
		border-radius: 50%;
		width: 32px;
		height: 32px;
		cursor: pointer;
		font-size: 1.2rem;
		font-weight: bold;
		transition: all 0.2s;
	}

	.delete-btn:hover {
		background: #ff3838;
		transform: scale(1.1);
	}

	.empty-state {
		text-align: center;
		padding: 3rem 1rem;
		color: rgba(0, 0, 0, 0.6);
		font-size: 1.1rem;
	}

	.actions {
		text-align: center;
		margin-bottom: 2rem;
	}

	.clear-btn {
		background: #666;
		color: white;
		border: none;
		padding: 0.7rem 1.5rem;
		border-radius: 6px;
		cursor: pointer;
		transition: background 0.2s;
	}

	.clear-btn:hover {
		background: #555;
	}

	.stats {
		display: flex;
		justify-content: space-around;
		background: rgba(255, 255, 255, 0.1);
		padding: 1.5rem;
		border-radius: 8px;
		margin-top: 2rem;
	}

	.stat {
		text-align: center;
	}

	.stat-number {
		display: block;
		font-size: 2rem;
		font-weight: bold;
		color: var(--color-theme-1);
	}

	.stat-label {
		font-size: 0.9rem;
		color: rgba(0, 0, 0, 0.7);
	}

	@media (max-width: 640px) {
		.add-todo {
			flex-direction: column;
		}

		.filters {
			justify-content: center;
		}

		.todo-content {
			flex-direction: column;
			align-items: flex-start;
			gap: 0.5rem;
		}

		.todo-date {
			margin: 0;
			align-self: flex-end;
		}
	}
</style>